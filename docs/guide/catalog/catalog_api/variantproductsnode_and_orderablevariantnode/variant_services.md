# Variant Services

## VariantService

The goal of this service is to return [OrderableVariantNode](../productnode_and_orderableproductnode.md) based on [VariantProductNode](../productnode_and_orderableproductnode.md) and variantCode. This service also returns all available variant codes for the [VariantProductNode](../productnode_and_orderableproductnode.md).

``` php
/** @var VariantService $variantService */
$variantService = $this->get('silver_catalog.variant_service');
```

#### Service methods

|Method|Usage|Parameters|Return|
|--- |--- |--- |--- |
|createOrderableProductFromVariant|Returns OrderableVariantNode from VariantProductNode,</br>so it can be added into basket.|VariantProductNode $node</br>string $variantCode|OrderableVariantNode|
|getVariantInformation|Returns available variant codes for each given characteristic.</br>If variant is orderable it returns also it's code.|VariantProductNode $variantProduct</br>array $variants = array()|array()|


##### Usage

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

The VariantSortService should return the product variants in an ordered form, so they can be easily shown in the template.

``` php
/** @var $sortService \Silversolutions\Bundle\EshopBundle\Services\VariantSortService */
$sortService = $this->container->get('silver_catalog.variants_sort_service');
```

##### Service methods

|Method|Usage|Parameters|Return|Twig method|
|--- |--- |--- |--- |--- |
|sortCharacteristicCodes|sorts the characteristic codes|array $characteristicCodes</br>$characteristicIndex|array()|sort_characteristic_codes()|
|sortCharacteristics|sorts characteristics|array $characteristics</br>$type</br>$order|array()|sort_characteristics()|
|getCharacteristicsForB2B|get information for B2B variant table|VariantProductNode $catalogElement</br>array $order|array()||

##### Usage

``` 
{% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement.type) %}

{% set variantCharacteristic = variantCharacteristic|sort_characteristic_codes(variantIndex) %}
 
{% set characteristicsB2B = get_characteristics_b2b(catalogElement) %}
```

## How to define sorting for variants

eZ Commerce provides the ability to configure sorting of variant characteristics and sorting within one characteristic.

#### Sorting of characteristics

If you want to change an order for specific catalog element type (e.g. vegetable), so e.g. Color is displayed as a first option you can use configuration for it.

``` yaml
#sorting order for variant characteristics
#e.g. vegetable is the catalog element type
silver_eshop.default.catalog_type:
    vegetable:
        characteristics_order: ["1","2","3"],
```

!!! note

    The type of Catalog Element is set by the CatalogFactory, so if you are using different types for your products, you have to extend the configuration.

    ```
    silver_eshop.default.catalog_type:
        notebook:
            characteristics_order: ["1", "2"]
        monitor:
        characteristics_order: ["2", "3"]
        ```

#### Sorting within one characteristic

silver-e.shop provides configuration to sort the codes within one characteristic.

By default all characteristics are already sorted with ASC order. 

##### Example

- Colors will not be sorted (default order from eZ Matrix will be used)
- Width will be sorted in DESC order.

``` yaml
# by default the list of all variant codes is sorted alphabetically in the ASC order
# allowed values for order: ASC, DESC
# allowed values for sort: true, false
silver_eshop.default.sort_variant_codes:
    1:
        sort: false
    2:
        order: DESC
```
