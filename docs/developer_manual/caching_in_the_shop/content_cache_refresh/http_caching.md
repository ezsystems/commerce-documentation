#  HTTP caching 

A central point of HTTP caching is a service called **HttpCachingStrategyService**.

It allows to configure each controller's response separately.

Another important solution is **Identity\\IdentityDefiner**, **** which has separate caches for different users or user groups (similar to cache block keys concept in eZ publish 4). This service is optional.

## How to configure

First of all enable HTTP caching and ESI support and configure services in Symfony

Be sure that http caching is enabled in web server virtual host configuration:

``` 
SetEnv USE_HTTP_CACHE 1
```

Next enable ESI support in Symfony:

``` 
framework:
    esi: { enabled: true }
    fragments: { path: /_fragment }
```

The service configuration is handled in services.xml file:

``` 
        <service id="siso_core.http_caching.strategy" class="%siso_core.http_caching.strategy.class%">
            <argument type="service" id="ezpublish.config.resolver" />
        </service>
```

**Optional step:** configure identity definer service:

``` 
        <service id="siso_core.http_caching.identity_definer" class="%siso_core.http_caching.identity_definer.class%">
            <argument type="service" id="ezpublish.api.repository" />
            <tag name="ezpublish.identity_definer" />
        </service>
```

### Cache blocks configuration

The main idea is to configure sets of caching parameters (cache blocks) and use the name of cache blocks in controller actions:

``` 
$response = new Response();
// Get an instance of HTTP caching strategy
$cachingStrategy = $this->get('siso_core.http_caching.strategy');

// Cache response using preconfigured cache block name "product"
$cachingStrategy->defineResponse($response, 'product', $catalogElement->cacheIdentifier);
```

All cache blocks are described inside **silver\_eshop.default.http\_cache** parameter in parameters.yml:

``` 
    silver_eshop.default.http_cache:
        # cache block name
        any_cache_block_name:
            # Response max age in seconds. Zero means that this response will not be cached.
            max_age: 3600
            # Vary is a criteria that allows to have different caches for different users or user groups.
            # Possible values:
            #     ~          : don't vary cache. Let's have one page representation for everybody.
            #     cookie     : each user will have his own cache block for this response
            #     user-hash  : each user group will have its own cache based on IdentityDefiner.
            vary: ~
```

Below is example configuration of 4 different blocks:

``` 
    silver_eshop.default.http_cache:
        product:
            max_age: 3600
            vary: ~
        product_list:
            max_age: 3600
            vary: ~
        price_block:
            max_age: 0
        header_login:
            max_age: 36000
            vary: cookie
        basket_preview:
            max_age: 36000
            vary: cookie
```

## How to use 

As it is already shown above to cache responses we need to inject an instance of HttpCachingStrategyService or to get this instance from container. 

Symfony HTTP cache or Varnish will use response headers to store response. 

Method **defineResponse** reads parameters from cache blocks configuration and defines unique cache that gives a possibility to purge particular cache blocks matched by this ID. 

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>defineResponse </td>
<td><p>Response $response,</p>
<p>string $blockName,</p>
<p>$id = null</p></td>
<td><p>response object which will be modified</p>
<p>Cache block name</p>
<p>Cache identifier (e.q, basket id or catalog element id)</p></td>
</tr>
</tbody>
</table>

Example

``` 
$cachingStrategy->defineResponse($response, 'product', $catalogElement->cacheIdentifier);
```

## Purging caches

### Console command

<table>
<thead>
<tr class="header">
<th>Command</th>
<th>Parameters</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>silversolutions:http-cache:purge</td>
<td>ids</td>
<td><p>List of cache identifiers separated with a space. E.g. "111 222 333"</p>
<p>By default command will clear all http caches</p></td>
</tr>
</tbody>
</table>

Example

``` 
Usage:
  silversolutions:http-cache:purge [<ids>]...
Arguments:
  ids                            List of cache identifiers separated with a space. E.g. "111 222 333"
php bin/console silversolutions:http-cache:purge 1056 222 --env="prod"
```

### Purging using eZ Platform service

If you programmatically updated product data or basket or something else that is already cached, you need to update caches to show users fresh information.

To do it you should inject or get purger service:

**Example**

``` 
            <call method="setHttpCachePurger">
                <argument type="service" id="ezplatform.http_cache.purge_client" />
            </call>
```

...and call it

**Example**

``` 
    /**
     * @param GatewayCachePurger $cachePurger
     */
    public function (EzSystems\PlatformHttpCacheBundle\PurgeClient\PurgeClientInterface $cachePurger)
    {
        $this->cachePurger = $cachePurger;
    }
    /**
     * @param $basketId
     */
    protected function purgeBasketHttpCache($basketId)
    {
        if($this->cachePurger){
            $this->cachePurger->purge(['siso_basket' . $basketId]);
        }
    }
```

## Caching in CatalogController

Caching for different kinds of catalog elements is implemented in the CatalogController:

**silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Controller/CatalogController.php**

``` 
if ($catalogElement instanceof ProductNode) {
    $cachingStrategy->defineResponse($response, 'product', $catalogElement->cacheIdentifier);
} elseif ($catalogElement instanceof ProductType) {
    $cachingStrategy->defineResponse($response, 'product_type', $catalogElement->cacheIdentifier);
} elseif ($catalogElement instanceof CatalogElement
    && $this->getCustomerProfileDataService()->isUserAnonymous()
) {
    $cachingStrategy->defineResponse($response, 'product_list', $catalogElement->cacheIdentifier);
}
```
