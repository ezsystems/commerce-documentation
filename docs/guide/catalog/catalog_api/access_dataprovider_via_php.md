# Access dataprovider via PHP

## Catalog Data Provider Service

Example usage in Controller or services

``` 
/** @var $catalogService \Silversolutions\Bundle\EshopBundle\Services\Catalog\CatalogDataProviderService */
$catalogService = $this->get('silver_catalog.data_provider_service');

/** @var $catalogElement \Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement */
/* Get product by Url/Request */
$catalogElement = $catalogService->getDataProvider()->lookupByUrl($urlService->getSeoUrl($request), $ezHelper->getCurrentLanguageCode()); 
/* Get product by sku  */
$catalogElement = $catalogService->getDataProvider()->fetchElementBySku(
    $productNodeSku,
    array()
);
```

## Definition

Injecting different Data Providers to the Catalog Service using external configuration.

We have to be able to choose the provider depending on siteaccess, url etc.

## Implementation

To choose proper catalog provider we use tags and compiler pass. Documentation in : <http://symfony.com/doc/current/components/dependency_injection/tags.html>

### Compiler Pass

New compiler pass (CatalogDataProviderOperationsPass.php) collects all services, which are tagged with "catalog_data_provider_operation". Example below:

`<tag name="catalog_data_provider_operation" alias="ez5" />`

[ ](http://symfony.com/doc/current/components/dependency_injection/tags.html)

Compiler Pass calls "setDataProviderService" from CatalogDataProviderService.php and sets all available providers.

When actual call to catalog data provider service is made then depending on siteaccess the proper provider is chosen.

``` 
/**
* Array, which holds the catalog data providers services as dependencies.
*
* @var CatalogDataProvider[]
*/
protected $dataProviders;
 
...
 
public function getDataProvider()
{
    // return the data provider by siteaccess
    return $this->dataProviders[$this->configResolver->getParameter('catalog_data_provider', 'silver_eshop')];
}
```
