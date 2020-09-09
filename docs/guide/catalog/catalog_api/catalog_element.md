# Catalog element

The `CatalogElement` class defines the generic product and category model.

All parts of the catalog, such as products, product categories or product types inherit from `CatalogElement`.

## Properties

Each `CatalogElement` has predefined properties. These methods are validated automatically by the `validateProperties()` method.

|Identifier|Type|Description|
|--- |--- |--- |
|`name`|string|The name of the element|
|`text`|string|A short introduction text for the element|
|`image`|ImageField (FieldInterface)|An image for the element|
|`path`|array|The path to the element (array of identifiers)|
|`url`|string|The internal URL of the element. This URL should not be used for generating links. Use `seoUrl` instead|
|`seoUrl`|string|The human-readable URL of the element|
|`permanentUrl`|string|The internal permanent URL|
|`parentElementIdentifier`|string|The unique identifier of the parent element|
|`identifier`|string|The unique identifier|
|`dataMap`|FieldInterface[]|A list of Fields|
|`cacheIdentifier`|int\|string|Cache identifier of the element to use as key in cache storage|

There are four public methods to set the properties: 

- `setImage()`
- `setName()`
- `setText()`
- `setCacheIdentifier()`

### Validators

The following validators can be used when attributes are set in `CatalogElement`:

|Name|Parameters|Description|
|--- |--- |--- |
|`validateStringAttribute`|`$value`,</br>`$attribute`|Checks if the value is a valid string|
|`validateBooleanAttribute`|`$value`,</br>`$attribute`|Checks if the value is a valid boolean|
|`validateFloatAttribute`|`$value`,</br>`$attribute`|Checks if the value is a valid float|
|`validateIntegerAttribute`|`$value`,</br>`$attribute`|Checks if the value is a valid integer|
|`validateFieldAttribute`|`$value`,</br>`$attribute`,</br>`$fieldType`|Checks if the value is of given Field Type|
|`validateArrayAttribute`|`$value`,</br>`$attribute`|Checks if the value is an array|
|`validateArrayOfAttribute`|`$value`,</br>`$attribute`,</br>`$class`|Checks if the value is an array of specified class (interface)|

Concrete implementations of the `CatalogElement` class require you to extend `validateProperties()`.

## Fetching a catalog element

``` php
/** @var $catalogService \Silversolutions\Bundle\EshopBundle\Services\Catalog\CatalogDataProviderService */
$catalogService = $this->get('silver_catalog.data_provider_service');
try {
    /** @var EzHelperService $ezHelper */
    $ezHelper = $this->get('silver_tools.ez_helper');
    /** @var $catalogElement \Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement */
    $catalogElement = $catalogService->getDataProvider()->fetchElementByIdentifier(
        200,
        $ezHelper->getCurrentLanguageCode()
    );
 } catch (\Exception $e) {
    return $this->render(
        'SilversolutionsEshopBundle:Catalog:exception.html.twig',
        array(
            'exception' => $e
        ),
        $response
    );
}
```

## Fetching a list of catalog elements

The `fetchChildrenList()` method enables listing `CatalogElement` or `ProductNode` objects for a given Location ID.

The following example fetches products directly assigned to a `CatalogElement` starting from the first element (`offset=0, limit=100`).
The shop uses the language of the current SiteAccess.

``` php
$catalogList = $catalogService->getDataProvider()
        ->fetchChildrenList($locationId, 1, array(), null, 0, 100);
```

The following example fetches a list of products using a `productList` filter.

``` php
$catalogList = $catalogService->getDataProvider()
            ->fetchChildrenList($locationId, 1, array('filterType' => 'productList'), null, 0, 100);
```
