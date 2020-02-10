#  Routing system 
## Motivation

Since the eZ Commerce relies on the eZ Platform Content Management System, sometime the underlying routing rules of both systems cause conflicts. Also it is possible that the eZ Platform API stack is needed in eZ Commerce routes. In order to deal with this, an own route- / URL-matching solution was created.

If a product url is not routed correctly (e.g. because of complex matching rules) it is possible to setup a direct routing for products. Please note that the first part of the URL (here "products") could be translated and in this case the following routing rules has to be setup for each translation.

``` 
product_route:
    path: /products/{url}
    defaults:
        _controller: SilversolutionsEshopBundle:Catalog:show
        url: /
    requirements:
        url: ".+"
```

## Default Router

Namespace

Silversolutions\\Bundle\\EshopBundle\\Routing\\StandardRouter

There is a default routing system, that checks each required URL.

If the URL belongs to the [catalog element](23560458.html), or [silver module](silver.module_23560489.html), the router become active and redirect the user to the appropriate controller.

Class description:

This router is a derived class from a chained router implementation. Thus it tries do match the URL to its own specifications. If no matching route could be found, it will pass the request back to the router chain in order to leave matching to another instance.

Route matching priority:

1.  If the request is in legacy mode, return and leave route matching to another instance.
2.  Assume the request of a catalog object and try to find a responsible route by using the catalog data provider.
3.  Assume the request of a catalog object and try to find a route by using the navigation service.
4.  Assume the request of a silver.module object and try to find a route by using the navigation service.
5.  Assume the request of a PIM object and try to find a route by using the catalog data provider.
6.  Assume the request of a PIM object and try to find a route by using the navigation service.
7.  If no route could be found at all, the route matching is left to another instance.

### Priority

The Router is defined with the priority of 280, so you can still add your own chain router, that will be executed before.

**services.xml**

``` 
<parameter key="ses_routers.standard_router.class">Silversolutions\Bundle\EshopBundle\Routing\StandardRouter</parameter>

<!-- routers -->
<service id="ses_routers.standard_router" class="%ses_routers.standard_router.class%">
    <argument type="service" id="ezpublish.api.service.location"/>
    <argument type="service" id="ezpublish.api.service.url_alias"/>
    <argument type="service" id="ezpublish.api.service.content"/>
    <argument type="service" id="ezpublish.urlalias_generator"/>
    <argument type="service" id="router.request_context" />
    <argument type="service" id="logger" />
    <call method="setConfigResolver">
        <argument type="service" id="ezpublish.config.resolver.chain"/>
    </call>
    <call method="setServices">
        <argument type="service" id="siso_core.catalog_helper" />
    </call>
    <tag name="router" priority="280" />
</service>
```

## Usage of the navigation service

In order to determine if the URL belongs to the catalog or [silver module](silver.module_23560489.html), the [navigation service](/pages/createpage.action?spaceKey=EZC14&title=Navigation+%28deprecated%29&linkCreation=true&fromPageId=23560569) or the [catalog data provider](Access-dataprovider-via-PHP_23560560.html) is used.

Additionally the router uses the navigation service in order to set the [URL Mapping](Access-dataprovider-via-PHP_23560560.html) in order to set the proper URL.
