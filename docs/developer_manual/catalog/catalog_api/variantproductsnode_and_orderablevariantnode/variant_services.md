#  Variant Services 

## VariantService

The goal of this service is to return [OrderableVariantNode](23560374.html) based on [VariantProductNode](23560374.html) and variantCode. This service also returns all available variant codes for the [VariantProductNode](23560374.html).

``` 
/** @var VariantService $variantService */
$variantService = $this->get('silver_catalog.variant_service');
```

#### Service methods

<table>
<thead>
<tr class="header">
<th>Method</th>
<th>Usage</th>
<th>Parameters</th>
<th>Return</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>createOrderableProductFromVariant</code></pre></td>
<td><pre><code>Returns OrderableVariantNode from VariantProductNode,</code></pre>
<pre><code>so it can be added into basket.</code></pre></td>
<td><pre><code>VariantProductNode $node</code></pre>
<pre><code>string $variantCode</code></pre></td>
<td><pre><code>OrderableVariantNode</code></pre></td>
</tr>
<tr>
<td><pre><code>getVariantInformation</code></pre></td>
<td><pre><code>Returns available variant codes for each given characteristic.</code></pre>
<pre><code>If variant is orderable it returns also it&#39;s code.</code></pre></td>
<td><pre><code>VariantProductNode $variantProduct</code></pre>
<pre><code>array $variants = array()</code></pre></td>
<td><pre><code>array()</code></pre></td>
</tr>
</tbody>
</table>

##### Usage

``` 
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

``` 
/** @var $sortService \Silversolutions\Bundle\EshopBundle\Services\VariantSortService */
$sortService = $this->container->get('silver_catalog.variants_sort_service');
```

##### Service methods

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Usage</th>
<th>Parameters</th>
<th>Return</th>
<th>Twig method</th>
</tr>
</thead>
<tbody>
<tr>
<td>sortCharacteristicCodes</td>
<td><p>sorts the characteristic codes</p></td>
<td><p>array $characteristicCodes</p>
<p>$characteristicIndex</p></td>
<td>array()</td>
<td>sort_characteristic_codes()</td>
</tr>
<tr>
<td>sortCharacteristics</td>
<td>sorts characteristics</td>
<td><p>array $characteristics<br />
$type<br />
$order</p></td>
<td>array()</td>
<td>sort_characteristics()</td>
</tr>
<tr>
<td>getCharacteristicsForB2B</td>
<td>get information for B2B variant table</td>
<td><p>VariantProductNode $catalogElement</p>
<p>array $order</p></td>
<td>array()</td>
<td><br />
</td>
</tr>
</tbody>
</table>

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

``` 
#sorting order for variant characteristics
#e.g. vegetable is the catalog element type
silver_eshop.default.catalog_type:
    vegetable:
        characteristics_order: ["1","2","3"],
```

The type of Catalog Element is set by the [CatalogFactory](#) , so if you are using different types for your products, you have to extend the configuration.

    silver_eshop.default.catalog_type:
        notebook:
            characteristics_order: ["1", "2"]
        monitor:

``` 
        characteristics_order: ["2", "3"]
```

#### Sorting within one characteristic

silver-e.shop provides configuration to sort the codes within one characteristic.

By default all characteristics are already sorted with ASC order. 

##### Example

  - Colors will not be sorted (default order from eZ Matrix will be used)
  - Width will be sorted in DESC order.

``` 
# by default the list of all variant codes is sorted alphabetically in the ASC order
# allowed values for order: ASC, DESC
# allowed values for sort: true, false
silver_eshop.default.sort_variant_codes:
    1:
        sort: false
    2:
        order: DESC
```
