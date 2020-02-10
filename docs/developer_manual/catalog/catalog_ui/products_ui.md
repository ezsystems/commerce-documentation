#  Products - UI 

## Data provided for products

In addition to the list of fields provided for a catalog, additional fields are provided for each product. Please see `Product category (CatalogElement)` and `ProductNode` for a detailed documentation.

**ProductNode & OrderableProductNode**

**Predefined properties for ProductNode**

Each `ProductNode` has predefined properties. These methods are validated automated on constructor by the `validateProperties()` method.

<table>
<thead>
<tr class="header">
<th>Identifier</th>
<th><p>Identifier</p>
<p>eZ</p></th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>name</td>
<td>ses_name</td>
<td>string</td>
<td>Product name</td>
</tr>
<tr>
<td><code>sku</code></td>
<td>ses_sku</td>
<td><code>string</code></td>
<td>Unique Stock Keeping Unit (SKU) of a product</td>
</tr>
<tr>
<td><code>manufacturerSku</code></td>
<td>ses_manufacturer_sku</td>
<td><code>string</code></td>
<td>Stock Keeping Unit (SKU) by a manufacturer of a product</td>
</tr>
<tr>
<td><code>ean</code></td>
<td>ses_ean</td>
<td><code>string</code></td>
<td>European Article Number (EAN) of a product</td>
</tr>
<tr>
<td><pre><code>type</code></pre></td>
<td>ses_product_type</td>
<td><pre><code>string</code></pre></td>
<td><p>Type of a product, e.g. vegetable</p></td>
</tr>
<tr>
<td><code>isOrderable</code></td>
<td><br />
</td>
<td><code>boolean</code></td>
<td>True, if a product is orderable</td>
</tr>
<tr>
<td><code>price</code></td>
<td>ses_unit_price</td>
<td><code>FieldInterface</code></td>
<td>Price of a product</td>
</tr>
<tr>
<td><code>customerPrice</code></td>
<td><br />
</td>
<td><code>PriceField (FieldInterface)</code></td>
<td>Customer price of a product which might be generated from a <code>PriceProvider</code></td>
</tr>
<tr>
<td><pre><code>scaledPrices</code></pre></td>
<td><br />
</td>
<td><pre><code>ArrayField</code></pre></td>
<td>An array with scaled prices and parameters to determine which scale price should be applied</td>
</tr>
<tr>
<td><pre><code>stock</code></pre></td>
<td>ses_stock_numeric</td>
<td><code>FieldInterface</code></td>
<td>Returns the stock of  a product</td>
</tr>
<tr>
<td>subtitle</td>
<td>ses_subtitle</td>
<td>TextBlockField (FieldInterface)</td>
<td><br />
</td>
</tr>
<tr>
<td><code>shortDescription</code></td>
<td><pre><code>ses_short_description</code></pre></td>
<td><code>TextBlockField (FieldInterface)</code></td>
<td>Short description of a product</td>
</tr>
<tr>
<td><code>longDescription</code></td>
<td>ses_long_description</td>
<td><code>TextBlockField (FieldInterface)</code></td>
<td>Long description of a product</td>
</tr>
<tr>
<td><code>specifications</code></td>
<td>ses_specifications</td>
<td><code>FieldInterface[]</code></td>
<td>A list of specifications of a product</td>
</tr>
<tr>
<td><code>imageList</code></td>
<td>ses_image_1 .. ses_image_4</td>
<td><code>ImageField[] (FieldInterface[])</code></td>
<td>A list of images</td>
</tr>
<tr>
<td><pre><code>minOrderQuantity</code></pre></td>
<td>ses_min_order_quantity</td>
<td><pre><code>float</code></pre></td>
<td>Minimal order quantity of the product</td>
</tr>
<tr>
<td><pre><code>maxOrderQuantity</code></pre></td>
<td>ses_max_order_quantity</td>
<td><pre><code>float</code></pre></td>
<td>Maximal order quantity of the product</td>
</tr>
<tr>
<td><pre><code>allowedQuantity</code></pre></td>
<td><br />
</td>
<td><pre><code>string</code></pre></td>
<td><div class="content-wrapper">
<p>Regex that indicates the allowed quantity</p>
<ul>
<li>this regex can be evaluated using the preg_match() function by some processes to check if the given quantity corresponds to the allowed quantity</li>
<li>values of ProductNodeConstants::ALLOWED_QUANTITY_* can be used</li>
</ul>
<p>Possible constants:</p>
<ul>
<li>ALLOWED_QUANTITY_INTEGER</li>
<li>ALLOWED_QUANTITY_UP_TO_ONE_DECIMAL_PLACE</li>
<li>ALLOWED_QUANTITY_UP_TO_TWO_DECIMAL_PLACES</li>
<li>ALLOWED_QUANTITY_MULTIPLE_DECIMAL_PLACES</li>
</ul>
<pre><code>Example for an individual expression: &#39;#^[0-9]+(\.|,)?[0-9]*$#&#39;</code></pre>
<p>If not set the quantity can be int only!</p>
<p>Example:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: RDark" data-theme="RDark"><code>&#39;allowedQuantity&#39; =&gt;  ProductNodeConstants::ALLOWED_QUANTITY_UP_TO_ONE_DECIMAL_PLACE,</code></pre>
</td>
</tr>
<tr>
<td><pre><code>packagingUnit</code></pre></td>
<td>ses_packaging_unit</td>
<td><pre><code>float</code></pre></td>
<td>Packaging unit of the product</td>
</tr>
<tr>
<td><pre><code>unit</code></pre></td>
<td>ses_unit</td>
<td><pre><code>string</code></pre></td>
<td>A unit of the product</td>
</tr>
<tr>
<td><pre><code>vatCode</code></pre></td>
<td>ses_vat_code</td>
<td><pre><code>string</code></pre></td>
<td>VAT code of the product. Needed to determine VAT percent</td>
</tr>
</tbody>
</table>

For every property there is a public setter method

# Additional fields within the dataMap

In addition the fields within some properties for each catalog additional fields will be provided in the property **`dataMap`**. The `dataMap` will provide an array of objects which implements the `FieldInterface`. The Twig function `ses_render_field()` can automatically decide, either to use a property of the `CatalogElement` or a property from the `dataMap`. If you have a property within the `dataMap` which has the same name as another property of the `CatalogElement`, you can enforce using the property from the `dataMap` by using the "`enforce_datamap`" parameter.

**Example:**

Object `$catalogElement` has property `price` (which has a `PriceField` with value "1.00") and a `PriceField` "price" (value "2.00") within its `dataMap`.

``` 
{# "1.00" is outputted #}
{{ ses_render_field(catalogElement, 'price') }}
 
{# "2.00" is outputted #}
{{ ses_render_field(catalogElement, 'price', {"enforce_datamap": true}) }}
```

## Attributes of ProductNode

Here are some Attributes, that already can be used in the template. These attributes are stored in $dataMap and can be used with {{ ses\_render\_field() }}.

In additon a product node provides the attributes of a CatalogElement (see [Product category (CatalogElement)](23560458.html)).

<table>
<thead>
<tr class="header">
<th>identifier</th>
<th>type</th>
<th>description</th>
<th>usage</th>
</tr>
</thead>
<tbody>
<tr>
<td>manufacturerSku</td>
<td>string</td>
<td>SKU of the product from manufacturer</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;manufacturerSku&#39;) }}</code></pre></td>
</tr>
<tr>
<td>specification</td>
<td>text block</td>
<td>Technical specification</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;specification&#39;) }}</code></pre></td>
</tr>
<tr>
<td>manufacturer</td>
<td>string</td>
<td><br />
</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;manufacturer&#39;) }}</code></pre></td>
</tr>
<tr>
<td>color</td>
<td>string</td>
<td>choosed color</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;color&#39;) }}</code></pre></td>
</tr>
<tr>
<td>video</td>
<td>string</td>
<td>The url for the video</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;video&#39;) }}</code></pre></td>
</tr>
<tr>
<td>image1</td>
<td>image</td>
<td>optional image</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;image1&#39;) }}</code></pre>
<pre><code>{{ catalogElement.dataMap.image1 }}</code></pre>
<pre><code>&lt;img src=&quot;/{{ catalogElement.dataMap.image1.path }}&quot; /&gt;</code></pre></td>
</tr>
<tr>
<td>image2</td>
<td>image</td>
<td>optional image</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;image2&#39;) }}</code></pre>
<pre><code>{{ catalogElement.dataMap.image2 }}</code></pre>
<pre><code>&lt;img src=&quot;/{{ catalogElement.dataMap.image2.path }}&quot; /&gt;</code></pre></td>
</tr>
<tr>
<td>image3</td>
<td>image</td>
<td>optional image</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;image3&#39;) }}</code></pre>
<pre><code>{{ catalogElement.dataMap.image3 }}</code></pre>
<pre><code>&lt;img src=&quot;/{{ catalogElement.dataMap.image3.path }}&quot; /&gt;</code></pre></td>
</tr>
<tr>
<td>image4</td>
<td>image</td>
<td>optional image</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;image4&#39;) }}</code></pre>
<pre><code>{{ catalogElement.dataMap.image4 }}</code></pre>
<pre><code>&lt;img src=&quot;/{{ catalogElement.dataMap.image4.path }}&quot; /&gt;</code></pre></td>
</tr>
<tr>
<td>specification</td>
<td>array</td>
<td>technical specifications</td>
<td><pre><code>{{ ses_render_field(catalogElement, &#39;specification&#39;) }}</code></pre></td>
</tr>
</tbody>
</table>

# Get a ProductNode by SKU

If fetching of a product in template is necessary (e.g. in basket), there is the Twig function `ses_product`.

``` 
{% set product = ses_product({'sku': 1100}) %}
SKU: {{ product.sku }}
Name: {{ product.name }}
Image: {{ ses_render_field(product, 'image1') }}
```

Twig `ses_product` parameters

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Mandatory</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>sku</code></td>
<td>yes</td>
<td>The SKU of the product node</td>
</tr>
<tr>
<td><code>sub_tree_path</code></td>
<td>no</td>
<td>An optional eZ sub tree path, to search product nodes (default: <code>/1/2</code> to search within complete tree)</td>
</tr>
<tr>
<td><pre><code>variantCode</code></pre></td>
<td>no</td>
<td>An optional parameter if a given variant shall be returned</td>
</tr>
</tbody>
</table>

# Images and assets

Images and assets can be taken from the CMS (if the dataprovider eZ is used) or external images can be used stored in the filesystem. 

If images from the file system shall be used they have to be stored in the following folders (the folders can be configured). In order to avoid to many files in one directory subfolders are used located under the following folders

  - Product images in web/var/assets/product\_images
  - Product group images web/var/assets/product\_group\_images

Example: 

  - SKU:  D4241

Files in the file system: 

``` 
web/var/assets/product_images/D/4/D4142_picture.jpg
web/var/assets/product_images/D/4/D4142_picture_2.jpg
```

When using a cluster this folder has to be mounted as a NFS share ore gluster FS filesystem. 

These folders are used as a source folders for all kind of assets.

Images will be resized automatically when they will be used for the first time. The different image variations will be stored in 

    web/var/ecommerce/storage/<image_class>/<subfolders>

``` 
web/var/ecommerce/storage//images/image_zoom/D/4/1/-D4142_picture_2_image_zoom.jpg
web/var/ecommerce/storage//images/image_zoom/D/4/1/-D4142_picture_image_zoom.jpg
web/var/ecommerce/storage//images/thumb_big/D/4/1/-D4142_picture_2_thumb_big.jpg
web/var/ecommerce/storage//images/thumb_big/D/4/1/-D4142_picture_thumb_big.jpg
web/var/ecommerce/storage//images/thumb_medium/D/4/1/-D4142_picture_thumb_medium.jpg
web/var/ecommerce/storage//images/thumb_medium/D/4/1/589768-D4142_thumb_medium.jpg
web/var/ecommerce/storage//images/thumb_smaller/D/4/1/-D4142_picture_2_thumb_smaller.jpg
web/var/ecommerce/storage//images/thumb_smaller/D/4/1/-D4142_picture_thumb_smaller.jpg
```

The image paths are cached in order to avoid accessing the filesystem in production mode. The cache is using stash. 

## Remove stash cache for images and one sku

``` 
 /** @var $catalogElement \Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement */
        $catalogElement = $catalogService->getDataProvider()->fetchElementBySku(
            $sku,
            array(),
            $ezHelper->getCurrentLanguageCode()
        );
 $assetService = $this->get('silver_catalog.asset_service');
 $assetService->removeAssetCache($catalogElement);
```
