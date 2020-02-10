#  ProductNode & OrderableProductNode 

The **`ProductNode`** is the abstract base class for product elements for all concrete product nodes. It inherits from `Product category (CatalogElement)`.

This class defines the base methods, which are needed to instantiate a product node within the tree structure. Also some important properties are predefined.

The `OrderableProductNode` is a concrete implementation for a product from class **`ProductNode`**. There are more concrete implementations available for `ProductNode`.

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
