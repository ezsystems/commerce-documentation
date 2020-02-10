#  Twig extension 

# Functions

<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Parameters</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>SilvercommonExtension</code></td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><code>ses_render_field</code></td>
<td><pre><code>CatalogElement $catalogElement</code></pre>
<pre><code>string $fieldIdentifier = &#39;&#39;</code></pre>
<pre><code>array $params = array()</code></pre></td>
<td>Renders an CatalogElement containing an Field</td>
</tr>
<tr>
<td><pre><code>ses_navigation</code></pre></td>
<td><pre><code>array $params = array()</code></pre></td>
<td>Returns navigation node tree</td>
</tr>
<tr>
<td><pre><code>ses_product</code></pre></td>
<td><pre><code>array $params = array()</code></pre></td>
<td><div class="content-wrapper">
<p>Returns a product node</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code> {% set product = ses_product({&#39;sku&#39;: line.sku, &#39;variantCode&#39;: line.variantCode }) %}</code></pre>
</td>
</tr>
<tr>
<td><pre><code>ses_variant_product_by_sku</code></pre></td>
<td><pre><code>string $sku</code></pre></td>
<td>Fetches a VariantProductNode for given sku</td>
</tr>
<tr>
<td><pre><code>ses_assets_main_image</code></pre></td>
<td><pre><code>CatalogElement $catalogElement</code></pre>
<pre><code>string $productId = null</code></pre></td>
<td>Returns the main image of catalogElement or null</td>
</tr>
<tr>
<td><pre><code>ses_assets_image_list</code></pre></td>
<td><pre><code>CatalogElement $catalogElement</code></pre>
<pre><code>$productId = null</code></pre></td>
<td>Returns the list of images for a catalogElement or an empty array</td>
</tr>
<tr>
<td><pre><code>ses_variants_sort</code></pre></td>
<td><pre><code>VariantProductNode $node</code></pre>
<pre><code>$order = array()</code></pre></td>
<td>Returns all product variants in a given order</td>
</tr>
<tr>
<td><pre><code>ses_render_price</code></pre></td>
<td><pre><code>CatalogElement $catalogElement</code></pre>
<pre><code>PriceField $field</code></pre>
<pre><code>array $params = array()</code></pre></td>
<td>Renders a <a href="PriceField_23560440.html">PriceField</a></td>
</tr>
<tr>
<td><pre><code>ses_render_stock</code></pre></td>
<td><pre><code>StockField $field = null,</code></pre>
<pre><code>array $params = array()</code></pre></td>
<td><p>Renders a <a href="StockField_23560549.html">StockField</a></p></td>
</tr>
<tr>
<td><pre><code>ses_basket</code></pre></td>
<td>-</td>
<td><p>Gets the basket of the current user</p></td>
</tr>
<tr>
<td><pre><code>ses_wish_list</code></pre></td>
<td>-</td>
<td>Gets the wishlist of the current user</td>
</tr>
<tr>
<td><pre><code>ses_total_comparison</code></pre></td>
<td>-</td>
<td><p>Get total basket lines array for basket flayout</p></td>
</tr>
<tr>
<td><pre><code>ses_check_product_in_comparison</code></pre></td>
<td><pre><code>$sku
$variantCode = null</code></pre></td>
<td><p>Returns true if product with given sku and variant code is already in the product comparison<br />
</p></td>
</tr>
<tr>
<td><pre><code>ses_check_product_in_wish_list</code></pre></td>
<td><pre><code>$sku
$variantCode = null</code></pre></td>
<td><p>Returns true if product with given sku and variant code is already in the wishList</p></td>
</tr>
<tr>
<td><pre><code>ses_pagination</code></pre></td>
<td><pre><code>Pagerfanta $pagerfanta</code></pre>
<pre><code>$viewName = null</code></pre>
<pre><code>array $options = array()</code></pre></td>
<td>Renders pagination</td>
</tr>
<tr>
<td><pre><code>get_stored_baskets</code></pre></td>
<td>-</td>
<td><p>Returns stored baskets for current user</p></td>
</tr>
<tr>
<td><pre><code>ses_user_menu</code></pre></td>
<td>-</td>
<td><p>Returns rendered user menu<br />
Combines parts of all configured bundles</p></td>
</tr>
<tr>
<td><pre><code>ses_track_basket</code></pre></td>
<td><pre><code>Basket $basket</code></pre>
<pre><code>$mode</code></pre>
<pre><code>array $params = array()</code></pre></td>
<td><p>Returns a list with rendered template snippets for basket tracking</p></td>
</tr>
<tr>
<td><pre><code>ses_track_product</code></pre></td>
<td><pre><code>ProductNode $catalogElement</code></pre>
<pre><code>$mode</code></pre>
<pre><code>array $params = array()</code></pre></td>
<td><p>Returns a list with rendered template snippets for product tracking</p></td>
</tr>
<tr>
<td><pre><code>ses_track_base</code></pre></td>
<td><pre><code>array $params = array()</code></pre></td>
<td><p>Returns a list with rendered template snippets for general tracking</p></td>
</tr>
<tr>
<td><pre><code>get_search_query</code></pre></td>
<td>-</td>
<td><p>Returns search query</p></td>
</tr>
<tr>
<td><pre><code>get_siteaccess_locale</code></pre></td>
<td><pre><code>$siteAccessName = null</code></pre></td>
<td><div class="content-wrapper">
<p>Returns the Symfony locale that matches the given siteaccess. if no siteAccessName is given, current siteaccess is taken.</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{# will return &#39;de-DE&#39; #}
{% set locale = get_siteaccess_locale(&#39;ger&#39;) %}</code></pre>
</td>
</tr>
<tr>
<td><pre><code>get_characteristics_b2b </code></pre></td>
<td><pre><code>VariantProductNode $catalogElement</code></pre>
<pre><code>array $order = array()</code></pre></td>
<td>Returns characteristic sorted for B2B</td>
</tr>
<tr>
<td><pre><code>has_user_role</code></pre></td>
<td><pre><code>$module</code></pre>
<pre><code>$function</code></pre></td>
<td>Checks if user has role</td>
</tr>
<tr>
<td><pre><code>get_parent_product_catalog</code></pre></td>
<td><pre><code>CatalogElement $catalogElement</code></pre></td>
<td><p>Fetches the parent product catalog element for the $identifier</p></td>
</tr>
<tr>
<td><pre><code>get_data_location_ids</code></pre></td>
<td><pre><code>$object</code></pre></td>
<td><p>Returns a comma separated string of data_locations of the given element.<br />
All parent locations incl. the element location are returned.</p></td>
</tr>
<tr>
<td><pre><code>set_bold_text</code></pre></td>
<td><pre><code>$needle</code></pre>
<pre><code>$haystack</code></pre></td>
<td><p>Returns html with added &lt;strong&gt; tag(s) to needle occurences in haystack.<br />
Supports multiple words.</p></td>
</tr>
<tr>
<td><pre><code>has_subcategory</code></pre></td>
<td><pre><code>$locationId</code></pre>
<pre><code>$offset</code></pre>
<pre><code>$limit</code></pre></td>
<td><p>Checks if the current category has subcategories</p></td>
</tr>
<tr>
<td><pre><code>get_customer_sku</code></pre></td>
<td><pre><code>$sku</code></pre>
<pre><code>$customerNumber</code></pre></td>
<td><p>Returns customer sku for given sku with customer number</p></td>
</tr>
<tr>
<td><pre><code>get_sku</code></pre></td>
<td><pre><code>$customerSku</code></pre>
<pre><code>$customerNumber = null</code></pre></td>
<td><p>Returns sku for given customer sku with customer number</p></td>
</tr>
<tr>
<td><pre><code>get_search_result_template</code></pre></td>
<td><pre><code>$catalogElement</code></pre></td>
<td><p>Selects the correct template according to catalog element type</p></td>
</tr>
<tr>
<td><pre><code>ses_fetch_content_by_identifier</code></pre></td>
<td><pre><code>string $contentType</code></pre>
<pre><code>string $identifier</code></pre></td>
<td>Fetches a content object with specific type and a field 'identifier' with a specific value.</td>
</tr>
<tr>
<td><pre><code>ses_config_parameter</code></pre></td>
<td><pre><code>$code, $domain, $scope = null</code></pre></td>
<td>Returns a siteaccess parameter from the configuration</td>
</tr>
<tr>
<td><pre><code>st_imageconverter</code></pre></td>
<td><pre><code>Abstract $imageFieldstring $size</code></pre></td>
<td>Provides an image in the required resolution</td>
</tr>
<tr>
<td><pre><code>st_siteaccess_lang</code></pre></td>
<td><pre><code>string $siteAccessName</code></pre></td>
<td>Returns the prioritiest language for given siteaccess</td>
</tr>
<tr>
<td><pre><code>st_tag</code></pre></td>
<td><pre><code>$component</code></pre>
<pre><code>$target</code></pre>
<pre><code>$action</code></pre>
<pre><code>$id = null</code></pre>
<pre><code>$value = null</code></pre></td>
<td><p>Returns the selector tag for Behat tests or for google tag management</p></td>
</tr>
</tbody>
</table>

# Filters

<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Paramters</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>date_format</code></pre></td>
<td><pre><code>$date</code></pre>
<pre><code>$inputFormat = &#39;Y-m-d&#39;</code></pre>
<pre><code>$locale = &#39;en&#39;</code></pre>
<pre><code>$dateType = &#39;\IntlDateFormatter::GREGORIAN&#39;</code></pre>
<pre><code>$timeType = &#39;\IntlDateFormatter::NONE&#39;</code></pre>
<pre><code>$formatPattern = &#39;dd.MM.yyyy&#39;</code></pre></td>
<td>Formats a DATE value</td>
</tr>
<tr>
<td><pre><code>price_format</code></pre></td>
<td><pre><code>$priceValue</code></pre>
<pre><code>$currency</code></pre>
<pre><code>$locale</code></pre></td>
<td><div class="content-wrapper">
<p>Formats a price value</p>

<p>default parameters in silver.eshop.yml</p>
<p>silver_eshop.default.price_format_currency: EUR</p>
<p>silver_eshop.default.price_format_locale: en</p>
</td>
</tr>
<tr>
<td><pre><code>youtube_video_id</code></pre></td>
<td><pre><code>$youtubeUrl</code></pre></td>
<td>Returns a video ID from a Youtube URL</td>
</tr>
<tr>
<td><pre><code>shipping</code></pre></td>
<td><pre><code>Basket $basket</code></pre></td>
<td>Returns the shipping costs from the basket or null</td>
</tr>
<tr>
<td><pre><code>code_label</code></pre></td>
<td><pre><code>$code,</code></pre>
<pre><code>$context = null</code></pre></td>
<td><p>Returns the translated code label. This can be e.g. country code or different code like mrs.<br />
if the context is 'country' first Symfony Intl Bundle is used to resolve the country code to country name otherwise just the translation for the code is returned</p></td>
</tr>
<tr>
<td><pre><code>basket_discounts</code></pre></td>
<td><pre><code>Basket $basket</code></pre></td>
<td>Returns the discounts from the basket</td>
</tr>
<tr>
<td><pre><code>basket_add_costs</code></pre></td>
<td><pre><code>Basket $basket</code></pre></td>
<td>Returns the additional costs from the basket</td>
</tr>
<tr>
<td><pre><code>basket_add_lines</code></pre></td>
<td><pre><code>Basket $basket</code></pre></td>
<td>Returns the additional lines from the basket</td>
</tr>
<tr>
<td><pre><code>code_label</code></pre></td>
<td><pre><code>$code</code></pre>
<pre><code>$context = null</code></pre></td>
<td><p>Returns the translated code label<br />
If the context is 'country' first Symfony Intl Bundle is used to resolve the country code to country name</p></td>
</tr>
<tr>
<td><pre><code>sort_characteristic_codes</code></pre></td>
<td><pre><code>array $characteristicCodes</code></pre>
<pre><code>$characteristicIndex</code></pre></td>
<td><p>By default the list of all variant codes is sorted alphabetically in the ASC order</p></td>
</tr>
<tr>
<td><pre><code>sort_characteristics</code></pre></td>
<td><pre><code>array $characteristics</code></pre>
<pre><code>$type</code></pre>
<pre><code>array $order = array()</code></pre></td>
<td><p>Returns sorted characteristics</p></td>
</tr>
<tr>
<td><pre><code>ses_format_args</code></pre></td>
<td><pre><code>$args</code></pre></td>
<td><p>Formats exception arguments</p></td>
</tr>
<tr>
<td><pre><code>truncate</code></pre></td>
<td><pre><code>$text</code></pre>
<pre><code>$limit</code></pre>
<pre><code>$break = &#39; &#39;</code></pre>
<pre><code>$pad = &#39;...&#39;</code></pre></td>
<td><p>Truncates the given text</p>
<p>$break parameter can be used to indicate where the truncate should break,e.g. words ($break = ' '), sentences ($break = '.')</p></td>
</tr>
<tr>
<td><pre><code>st_siteaccess_path</code></pre></td>
<td><pre><code>string $urlPath</code></pre>
<pre><code>string $siteAccessName = null</code></pre></td>
<td><p>formats the given url path into siteaccess path</p>
<p>e.g. home -&gt; /de/home</p></td>
</tr>
<tr>
<td><pre><code>st_siteaccess_url</code></pre></td>
<td><pre><code>string $urlPath</code></pre>
<pre><code>string $siteAccessName = null</code></pre></td>
<td><p>formats the given url path into siteaccess url</p>
e.g. home -&gt; <a href="http://harmony.silver-eshop.de/de/home" class="external-link">http://harmony.silver-eshop.de/de/home</a></td>
</tr>
<tr>
<td><pre><code>st_translate</code></pre></td>
<td><pre><code>$messageOrCode</code></pre>
<pre><code>$context = &#39;&#39;</code></pre>
<pre><code>$array $parameters = array()</code></pre>
<pre><code>$domain = null</code></pre>
<pre><code>$siteaccess = null</code></pre></td>
<td>Returns translation for $messageOrCode</td>
</tr>
<tr>
<td><pre><code>st_resolve_template</code></pre></td>
<td><pre><code>$templateName, $theme = null</code></pre></td>
<td><p>Returns resolved template name with the TemplateResolverService</p></td>
</tr>
</tbody>
</table>

# Global variables

There is a central global variable "`ses`" provided by the `EshopBundle`. In fact, this variable is an instance of **`GlobalVariables`** class which provides methods and is inherited from the Symfony global variable class. Therefore all methods from the global template variable "`app`" is also available in "`ses`".

|           |                                              |
| --------- | -------------------------------------------- |
| Class     | `GlobalVariables`                            |
| Namespace | ` Silversolutions\Bundle\EshopBundle\Twig  ` |

## Methods for global variable "ses"

<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Description</th>
<th>Example in template</th>
</tr>
</thead>
<tbody>
<tr>
<td><p><code>getProfile()</code></p>
<p><br />
</p>
<p>Aliases:</p>
<p><code>getProfileData()</code></p>
<p><code>getCustomerProfileData()</code></p>
<p><br />
</p></td>
<td>Returns the current customer profile</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>Current customer number: {{ ses.profile.sesUser.customerNumber }}</code></pre>
<p><strong>Logic:</strong></p>

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><code>ses</code></th>
<th><code>profile</code></th>
<th><code>sesUser</code></th>
<th><code>customerNumber</code></th>
</tr>
</thead>
<tbody>
<tr>
<td>Global Twig variable</td>
<td>public method <code>GlobalVariables::getProfile()</code></td>
<td><code>SesUser</code> from <code>CustomerProfileData</code> returned by <code>getProfile()</code></td>
<td><p>Readable variable from</p>
<p><code>SesUser-&gt;customerNumber</code></p></td>
</tr>
</tbody>
</table>

<p>Also see examples from <a href="Customers---Templating_23560911.html">Customers - Templating</a></p>
</td>
</tr>
<tr>
<td><code>getDefaultBuyerAddress()</code></td>
<td><p>Returns the default <code>BuyerParty</code></p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{% set buyerAddress = ses.defaultBuyerAddress %}
Street name of buyer address: {{ buyerAddress.PostalAddress.StreetName }}</code></pre>
</td>
</tr>
<tr>
<td><code>getDefaultDeliveryParty()</code></td>
<td>Returns the default <code>DeliveryParty</code></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{% set deliveryAddress = ses.defaultDeliveryParty %}
Street name of default delivery address: {{ buyerAddress.PostalAddress.StreetName }}</code></pre>
</td>
</tr>
<tr>
<td><code>getDeliveryParty($deliveryPartyId = null)</code></td>
<td><p>Returns <code>DeliveryParty</code> by <code>$deliveryPartyId</code>. If no <code>$deliveryPartyId</code> is given, all parties will be returned</p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{% set deliveryAddresses = ses.deliveryParty %}
{% for address in deliveryAddresses %}
  Street Number {{ loop.index }}: {{ deliveryAddress.PostalAddress.StreetName }}
{% endfor %}</code></pre>
</td>
</tr>
</tbody>
</table>
