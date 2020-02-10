#  Product Variants 

## General Information

[Variants](23560374.html) are used if a product is available in different options. eZ Commerce offers a flexible variant model allowing 1..3 levels of variant characteristics such as color or size. 

Features

eZ Commerce comes with an out-of-the-box support for variants for B2C and B2B shops. It displays variants in a different way, respectively, on the product detail page. For B2B it allows buying multiple variants in one step.

There is a parameter which determines this value by siteaccess.

``` 
# allowed values: B2B, B2C
# B2C - variants are displayed showing all characteristics
# B2B - variants are grouped for 1-step ordering
silver_eshop.default.variant_type: B2C
```

##### Icons

[Factory](How-to-setup-variants-from-external-source_23560313.html), that will creates variants, can also provide images for some characteristics. In our example the factory provided some images for the characteristoc 'Color'. If the images are provide, they are taken, otherwise just text is displayed in a box, see Unit and Width.

How the images should be stored in the variant characteristics:

``` 
    /**
     * 
     * array(
     *     'characteristic_identifier_1' => array(
     *         'label' => 'Label Value 1',
     *         'type'  => 'radio',
     *         'images' => array(
     *             'code_1' => 'Value 1',
     *             'code_2' => 'Value 2',
     *          ),
     *         'codes'  => array(
     *             'code_1' => 'Value 1',
     *             'code_2' => 'Value 2',
     *         ),
     *     ),
     *     'characteristic_identifier_2' => array(
     *         'label' => 'Label Value 2',
     *         'type'  => 'dropdown',
     *          'images' => array(
     *            'code_3' => 'Value 3',
     *          ),
     *         'codes'  => array(
     *             'code_3' => 'Value 3',
     *         ),
     *     ),
     * );
     * 
     */
```

if you just want to edit the images, or add new ones, see [How to adapt the frontend](#ProductVariants-front).

## Edit Variant in the basket

In the basket user has a possibility to change variant.  

![](attachments/23560478/23563305.png)

When clicking on the "pen" icon (image above) the popup will show with option to change variant. After save new variant will be stored in basket and basket will be recalculated.

![](attachments/23560478/23563319.png)![](attachments/23560478/23563320.png)

## How to adapt the frontend 

#### Templates used in product detail

**/vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/**

``` 
productBasketVariant.html.twig      - price block for variant
productData.html.twig               - product information (for variant can be additional info like country)
productDetailVariantB2B.html.twig   - display variant options for B2B
productDetailVariantB2C.html.twig   - display variant options for B2C
productVariantBlock.html.twig       - all logic for B2C display
```

#### Changing icon/image for variant characteristic like Color 

Let's assume that for "Color" characteristic we have codes like

  - grn
  - red
  - blu
  - org  

First we need to create images:

  - grn.jpg
  - red.jpg
  - blu.jpg
  - org.jpg  

Then we need to place them under "**bundles/silversolutionseshop/img/variants/**"

## How to define sorting for variants

eZ Commerce provides the ability to configure sorting of variant characteristics, please see the [VariantSortService](Variant-Services_23560238.html).

## How the prices are calculated

If variant is fully specified (all options are selected and variantCode is in place) there is an additional ajax call using phalanx to fetch real prices.

| Controller            | Method                      |
| --------------------- | --------------------------- |
| AjaxCatalogController | getPriceAction |

## Terms

The following terms are used in the eZ Commerce:

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Term
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
<th><div class="tablesorter-header-inner">
Example (Shoes)
</th>
</tr>
</thead>
<tbody>
<tr>
<td>VariantProduct(Node)</td>
<td>The virtual product, which contains the data for all variations that can be ordered. Since this product has no specific price nor stock which is assigned to its SKU, it can not be added to the basket directly.</td>
<td><p>SKU: 1234</p>
<p>ProductName: "RunnersX100"</p></td>
</tr>
<tr>
<td>OrderableVariant(Node)</td>
<td>This represents one specific, orderable variation. It is defined by its unique VariantCode or by the deterministic set of VariantCharacteristics. An OrderableVariantNode is intended to added to a Basket.</td>
<td><p>SKU: 1234</p>
<p>ProductName: "RunnersX100"</p>
<ul>
<li>Size: "EU 43"</li>
<li>Color: "Blue"</li>
</ul></td>
</tr>
<tr>
<td>VariantCode</td>
<td><p>This code identifies an OrderableVariant within a VariantProduct, thus it must be unique in the scope of the VariantProduct.</p></td>
<td><p>VariantCode: 43_blu</p>
<p>Would have:</p>
<ul>
<li>size: 43</li>
<li>color: blu</li>
</ul></td>
</tr>
<tr>
<td>VariantCharacteristic</td>
<td><p>One VariantCharacteristic is a specific attribute of a VariantProduct, which is distinctive and describes one aspect of the variant. A VariantProduct may have at least one VariantCharacteristic but also more than one.</p></td>
<td><ul>
<li>Color: "Blue", "Green", "Red"</li>
<li>Size: "EU 41", "EU 42", "EU 43"</li>
</ul></td>
</tr>
<tr>
<td>CharacteristicIdentifier</td>
<td><p>Class unique string for a characteristic. It is equivalent the identifier of an attribute.</p></td>
<td><ul>
<li>1: color</li>
<li>2: size</li>
</ul></td>
</tr>
<tr>
<td>CharacteristicLabel</td>
<td>This is a readable name for a characteristic. Its purpose is to be used for frontend output.</td>
<td>"Color" and "Size" are the labels in this example.</td>
</tr>
<tr>
<td>CharacteristicType</td>
<td>This is some kind of code, which is used to determine how the characteristic should be displayed / rendered.</td>
<td>'radio' could indicate, that all options should be put into a radiobox.</td>
</tr>
<tr>
<td>CharacteristicCode / CharacteristicValue</td>
<td>Every VariantCharacteristic can take several values. Like the CharacteristicLabel, the CharacteristicValue is a readable name for display purposes, but here for a single characteristic value. The CharacteristicCode is an unique string for that value, which is used to identify the individual values.</td>
<td>Color:<br />

<ul>
<li>Value: "Blue"</li>
<li>Code: blu</li>
</ul>
<p>Size:</p>
<ul>
<li>Value: "EU 43"</li>
<li>Code: 43</li>
</ul></td>
</tr>
<tr>
<td>VariantPriceRange</td>
<td>A set of two prices, which represent the lowest price of all variants and the highest.</td>
<td>43€ - 49€</td>
</tr>
</tbody>
</table>

Complete example for the terms:

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Product
</th>
<th><div class="tablesorter-header-inner">
SKU
</th>
<th><div class="tablesorter-header-inner">
Characteristics
</th>
<th><div class="tablesorter-header-inner">
Variants
</th>
</tr>
</thead>
<tbody>
<tr>
<td>"RunnersX100"</td>
<td>1234</td>
<td>
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
<th><div class="tablesorter-header-inner">
CharacteristicIdentifier
</th>
<th><div class="tablesorter-header-inner">
CharacteristicLabel
</th>
<th><div class="tablesorter-header-inner">
CharacteristicType
</th>
<th><div class="tablesorter-header-inner">
CharacteristicCode
</th>
<th><div class="tablesorter-header-inner">
CharacteristicValue
</th>
</tr>
</thead>
<tbody>
<tr>
<td>color</td>
<td>Color</td>
<td>radio</td>
<td>blu</td>
<td>Blue</td>
</tr>
<tr>
<td>color</td>
<td>Color</td>
<td>radio</td>
<td>grn</td>
<td>Green</td>
</tr>
<tr>
<td>size</td>
<td>Size</td>
<td>dropdown</td>
<td>41</td>
<td>EU 41</td>
</tr>
<tr>
<td>size</td>
<td>Size</td>
<td>dropdown</td>
<td>42</td>
<td>EU 42</td>
</tr>
<tr>
<td>size</td>
<td>Size</td>
<td>dropdown</td>
<td>43</td>
<td>EU 43</td>
</tr>
</tbody>
</table>
</td>
<td>
<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
VariantCode
</th>
<th><div class="tablesorter-header-inner">
CharacteristicCode (CharacteristicIdentifier: 1)
</th>
<th><div class="tablesorter-header-inner">
CharacteristicCode (CharacteristicIdentifier: 2)
</th>
</tr>
</thead>
<tbody>
<tr>
<td>blu_41</td>
<td>blu</td>
<td>41</td>
</tr>
<tr>
<td>blu_43</td>
<td>blu</td>
<td>43</td>
</tr>
<tr>
<td>grn_42</td>
<td>grn</td>
<td>42</td>
</tr>
<tr>
<td>grn_43</td>
<td>grn</td>
<td>43</td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2013-10-21 um 07.44.07.png](attachments/23560366/23563597.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2013-10-24 um 08.39.55.png](attachments/23560366/23563595.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-04-16 13:13:10.png](attachments/23560366/23563350.png) (image/png)  
