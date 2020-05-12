# Variant services

## VariantService

`VariantService` returns an [`OrderableVariantNode`](../productnode_and_orderableproductnode.md)
based on [`VariantProductNode`](../productnode_and_orderableproductnode.md) and `variantCode`.
It also returns all available variant codes for the [`VariantProductNode`](../productnode_and_orderableproductnode.md).

``` php
/** @var VariantService $variantService */
$variantService = $this->get('silver_catalog.variant_service');
```

### Service methods

|Method|Usage|Parameters|Returns|
|--- |--- |--- |--- |
|`createOrderableProductFromVariant`|Returns `OrderableVariantNode` from `VariantProductNode`, so it can be added to a basket.|`VariantProductNode $node`</br>`string $variantCode`|`OrderableVariantNode`|
|`getVariantInformation`|Returns available variant codes for each given characteristic. If a variant is orderable it also returns its code.|`VariantProductNode $variantProduct`</br>`array $variants = array()`|`array()`|

``` php
//   e.g. $variants = array(
//            'Color' => 'grn', 
//            'Unit' => 'box'
//        );

list($availableVariantCodes, $orderableVariantCode) =
    $variantService->getVariantInformation($catalogElement, $variants);
 
// create orderable variant node
$catalogElement = $variantService->createOrderableProductFromVariant($catalogElement, $variantCode);
```

## VariantSortService

`VariantSortService` returns the product variants in an ordered form, so they can be displayed in a template.

``` php
/** @var $sortService \Silversolutions\Bundle\EshopBundle\Services\VariantSortService */
$sortService = $this->container->get('silver_catalog.variants_sort_service');
```

### Service methods

|Method|Usage|Parameters|Return|Twig method|
|--- |--- |--- |--- |--- |
|`sortCharacteristicCodes`|Sorts the characteristic codes|`array $characteristicCodes`</br>``$characteristicIndex`|`array()`|`sort_characteristic_codes()`|
|`sortCharacteristics`|Sorts characteristics|`array $characteristics`</br>`$type`</br>`$order`|`array()`|sort_characteristics()`|
|`getCharacteristicsForB2B`|Gets information for B2B variant table|`VariantProductNode $catalogElement`</br>`array $order`|`array()`||

##### Usage

``` 
{% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement.type) %}

{% set variantCharacteristic = variantCharacteristic|sort_characteristic_codes(variantIndex) %}
 
{% set characteristicsB2B = get_characteristics_b2b(catalogElement) %}
```

## Defining sorting for variants

You can configure sorting of variant characteristics and sorting within one characteristic.

### Sorting of characteristics

If you want to change the order for a specific catalog element type (e.g. vegetable),
so that for example "Color" is displayed as a first option, use the following configuration:

``` yaml
#e.g. vegetable is the catalog element type
silver_eshop.default.catalog_type:
    vegetable:
        characteristics_order: ["1","2","3"],
```

!!! note

    The type of catalog element is set by the `CatalogFactory`, so if you are using different types for your products, you have to extend the configuration.

    ```
    silver_eshop.default.catalog_type:
        notebook:
            characteristics_order: ["1", "2"]
        monitor:
        characteristics_order: ["2", "3"]
        ```

### Sorting within one characteristic

By default all characteristics are already sorted in ASC order. 
 
You can configure other sort orders using `silver_eshop.default.sort_variant_codes`.

In the following example, colors are not sorted (default order from eZ Matrix is used)
and width is sorted in DESC order.

``` yaml
# allowed values for order: ASC, DESC
# allowed values for sort: true, false
silver_eshop.default.sort_variant_codes:
    1:
        sort: false
    2:
        order: DESC
```
