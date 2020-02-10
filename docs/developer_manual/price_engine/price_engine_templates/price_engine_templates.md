#  Price Engine - Templates 

### Templates list:

Default path:

    vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/
    EshopBundle/Resources/views

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Path</th>
<th>Accepted parameters</th>
<th>How to set the parameters</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>Catalog/Subrequests/product.html.twig</code></pre></td>
<td> </td>
<td><pre><code> </code></pre></td>
<td><ul>
<li>used on product detail page</li>
</ul>
<p>defines the parameters for price rendering and includes the <em>product_price.html.twig</em> template</p></td>
</tr>
<tr>
<td><pre><code>Catalog/listProductNode.html.twig</code></pre></td>
<td> </td>
<td><p>Example: VariantProductNode</p>
<p><img src="attachments/23560289/23563826.png" class="confluence-embedded-image" /></p></td>
<td><ul>
<li>used on product list page</li>
</ul>
<p>defines the parameters for price rendering and includes the <em>product_price.html.twig</em> template</p></td>
</tr>
<tr>
<td><pre><code>Catalog/Subrequests/product_price.html.twig</code></pre></td>
<td><p>{# parameters, that will be passed to the<br />
PriceField.html.twig  template to render the price #} </p>
<pre><code>&#39;renderParams&#39;: renderParams</code></pre>
<p>{# parameters, that will be used to design the label,<br />
e.g. 'List price', 'Your price' #} <br />
{# you can decide if you want to display the<br />
label and specify the css class #}<br />
'labelParams': labelParams <br />
</p>
<p>{# if set, the source will be displayed #}<br />
'displaySource' : true</p></td>
<td><pre><code>{% set labelParams = {
 &#39;priceRange&#39; : {
 &#39;cssClass&#39;: &#39;right_align block&#39;,
 &#39;show&#39;: true
 },
 &#39;customerPrice&#39;: {
 &#39;show&#39;: true
 },
 &#39;listPrice&#39;: {
 &#39;show&#39;: true
 }
 }
%}</code></pre></td>
<td><p>common template to render the price, display a label about the price type (e.g. 'list price') and display the price source (e.g. ERP)</p>
<p>includes <em>PriceField.html.twig</em> to render the price</p></td>
</tr>
<tr>
<td><pre><code>Fieldtypes/PriceField.html.twig</code></pre></td>
<td><pre><code>&#39;params&#39; : renderParams</code></pre>
<pre><code> </code></pre></td>
<td><pre><code>{% set renderParams = {
 &#39;outputPrice&#39;: {
 &#39;cssClass&#39;: &#39;price price_med&#39;,
 &#39;property&#39;: priceProperty
 },
 &#39;vatLabel&#39;: {
 &#39;cssClass&#39;: &#39;price&#39;,
 &#39;show&#39;: true,
 &#39;text&#39;: priceLabel
 }
 }
%}</code></pre></td>
<td>renders the given price from catalog element, see <a href="#PriceEngine-Templates-ses_render_price">ses_render_price</a></td>
</tr>
</tbody>
</table>

### Related custom Twig modifiers/functions/etc/:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Twig filter</th>
<th>Description</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>shipping</code></pre></td>
<td><pre><code>gets the list of shipping costs from the basket</code></pre></td>
<td><pre><code>{% set shippingCosts = basket|shipping %}</code></pre></td>
</tr>
<tr>
<td><pre><code>basket_discounts</code></pre></td>
<td><pre><code>gets the list of discounts from the basket</code></pre></td>
<td><pre><code>{% set discounts = basket|basket_discounts %}</code></pre></td>
</tr>
<tr>
<td><pre><code>basket_add_costs</code></pre></td>
<td><pre><code>gets the list of additional costs from the basket</code></pre></td>
<td><pre><code>{% set addCosts = basket|basket_add_costs %}</code></pre></td>
</tr>
<tr>
<td><pre><code>basket_add_lines</code></pre></td>
<td><pre><code>gets the list of additional lines from the basket</code></pre></td>
<td><pre><code>{% set addLines = basket|basket_add_lines %}</code></pre></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Twig function</th>
<th>Description</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>price_format</code></pre></td>
<td><pre><code>Formats a price value</code></pre></td>
<td><pre><code>{{ priceValue|price_format(currency, locale) }}</code></pre></td>
</tr>
<tr>
<td><pre><code>ses_render_price</code></pre></td>
<td><pre><code>Renders a PriceField from CatalogElement</code></pre></td>
<td><pre><code>{{ ses_render_price(catalogElement, minPrice,
 {
 &#39;outputPrice&#39;: {&#39;cssClass&#39;: &#39;price price_med&#39;}
 }
) }}</code></pre></td>
</tr>
</tbody>
</table>

#### <span id="PriceEngine-Templates-ses_render_price" class="confluence-anchor-link">ses\_render\_price

Following optional render parameters are available for a `PriceField`:

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Options</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>outputPrice</code></td>
<td>Enables actions on the output of the price value</td>
<td>
<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Default</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>id</code></td>
<td>Optional string of ID to use for price</td>
<td><code>""</code> (undefined)</td>
</tr>
<tr>
<td><code>cssClass</code></td>
<td>Optional string of CSS class(es) to use for price</td>
<td><code>""</code> (undefined)</td>
</tr>
<tr>
<td><code>locale</code></td>
<td>Two digit locale code (e.g.: "en", "us", "de")</td>
<td><code>"en"</code></td>
</tr>
<tr>
<td><code>currency</code></td>
<td>Three digit currency code (e.g.: EUR, GBP, USD)</td>
<td><code>price.currency</code></td>
</tr>
<tr>
<td><code>property</code></td>
<td>Property of price field used for outputted value</td>
<td><code>price.price</code></td>
</tr>
<tr>
<td><code>raw</code></td>
<td>Price is outputted without any HTML tags, if <code>true</code></td>
<td><code>false</code> (undefined)</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td><code>vatLabel</code></td>
<td>Enables actions on the optional VAT label</td>
<td>
<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Default</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>id</code></td>
<td>Optional string of ID to use for VAT label</td>
<td><code>""</code> (undefined)</td>
</tr>
<tr>
<td><code>show</code></td>
<td>VAT label is shown, if <code>true</code></td>
<td><code>false</code> (undefined)</td>
</tr>
<tr>
<td><code>cssClass</code></td>
<td>Optional string of CSS class(es) to use for VAT label</td>
<td><code>""</code> (undefined)</td>
</tr>
<tr>
<td><code>text</code></td>
<td>Override of the outputted text</td>
<td><p>Default text depending on <code>price.isVatPrice</code> ("Including VAT" or "Excluding VAT")</p></td>
</tr>
<tr>
<td><code>raw</code></td>
<td>VAT label is outputted without any HTML tags, if <code>true</code></td>
<td><code>false</code> (undefined)</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td>schema</td>
<td>enables to ouput schema infos</td>
<td><div class="content-wrapper">
<p>If set then a schema</p>
<pre><code>itemprop=&quot;price&quot; </code></pre>
<p>will be used:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;span itemprop=&quot;price&quot; content=&quot;1865.00&quot;&gt;1.865,00&amp;nbsp;€&lt;/span&gt;</code></pre>
</td>
</tr>
</tbody>
</table>

### Related routes:
