#  Search Autosuggest 

<table>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<img src="attachments/23560726/23563785.png" class="confluence-embedded-image" height="250" />
</td>
<td><div class="content-wrapper">

<ul>
<li><a href="#SearchAutosuggest-Thebuildinautosuggestfunctionprovidesauserfriendlywaytopreviewproducts,categories,contentordownloadswhichmatchesthegivensearchcriteria.">The build in autosuggest function provides a user friendly way to preview products, categories, content or downloads which matches the given search criteria. </a></li>
<li><a href="#SearchAutosuggest-IntroductionandFunctionality">Introduction and Functionality</a>
<ul>
<li><a href="#SearchAutosuggest-ModuleProduct">Module Product</a></li>
<li><a href="#SearchAutosuggest-ModuleCategory">Module Category</a></li>
<li><a href="#SearchAutosuggest-ModuleContent">Module Content</a></li>
<li><a href="#SearchAutosuggest-ModuleDownload">Module Download</a></li>
<li><a href="#SearchAutosuggest-Configuration">Configuration</a></li>
</ul></li>
<li><a href="#SearchAutosuggest-AutosuggestRedirectController">Autosuggest Redirect Controller</a></li>
<li><a href="#SearchAutosuggest-Cookbook">Cookbook</a>
<ul>
<li><a href="#SearchAutosuggest-Howtoaddadditionalelementstoautosuggestmodule">How to add additional elements to autosuggest module</a>
<ul>
<li><a href="#SearchAutosuggest-Example:adda%22new%22labeltocertainproducts">Example: add a "new" label to certain products</a></li>
</ul></li>
<li><a href="#SearchAutosuggest-Howtooverrideautosuggestservices">How to override autosuggest services</a></li>
</ul></li>
<li><a href="#SearchAutosuggest-Frontend:">Frontend:</a>
<ul>
<li><a href="#SearchAutosuggest-Whatyoushouldknowabouttemplatefilesandautosuggest">What you should know about template files and autosuggest</a>
<ul>
<li><a href="#SearchAutosuggest-Producttemplate">Product template</a></li>
<li><a href="#SearchAutosuggest-Categorytemplate">Category template</a></li>
<li><a href="#SearchAutosuggest-Contenttemplate">Content template</a></li>
<li><a href="#SearchAutosuggest-Downloadtemplate">Download template</a></li>
<li><a href="#SearchAutosuggest-AutosuggestProductTemplateFile">Autosuggest Product Template File</a></li>
<li><a href="#SearchAutosuggest-AutosuggestCategoryTemplateFile">Autosuggest Category Template File</a></li>
<li><a href="#SearchAutosuggest-AutosuggestContentTemplateFile">Autosuggest Content Template File</a></li>
<li><a href="#SearchAutosuggest-AutosuggestDownloadTemplateFile">Autosuggest Download Template File</a></li>
</ul></li>
<li><a href="#SearchAutosuggest-TwigFunctiontomarkas%3Cstrong%3Eusersearchterms">Twig Function to mark as &lt;strong&gt; user search terms</a></li>
<li><a href="#SearchAutosuggest-AdvancedJavascriptFunctionality">Advanced Javascript Functionality</a></li>
</ul></li>
</ul>

</td>
</tr>
</tbody>
</table>

## The build in autosuggest function provides a user friendly way to preview products, categories, content or downloads which matches the given search criteria. 

## Introduction and Functionality

As user types in main search field, different and configurable search results appear.

Autosuggest has 4 different independent modules:

And for each module the functionality is similar: 

1.  User types in search field.
2.  A Solr search is performed using the content of the search field directly to Solr using Solarium.
3.  Solr result is showed according to configuration..
4.  A different action is performed according to module type using a redirect controller that works after element is clicked:
    1.  A product click will redirect to product detail page.
    2.  A category click will redirect to category detail page.
    3.  A content click will redirect to Ez Content detail page.
    4.  A download click will start download of the current file.  

Note: It is very important to keep processing time as low as possible, since time is critical. That's why search is performed directly to Solr without any additional components from eZ Platform or from eZ Commerce.

### Module Product

This module is designed to display eZ Commerce products. Several fields can be showed according to configuration (see below: configuration).

Clicking on a product will redirect to the product detail page. Due to performance reasons the price is not displayed in this step. If a product has variants no addtobasket is offered. 

Also if Add to basket is set to true, an add to basket button will appear and the user will be able to add to basket directly from autosuggest window.

### Module Category

This module is designed to display eZ Commerce categories. Some fields can be showed according to configuration (see below: configuration).

Clicking on a category will redirect to the category detail page.

### Module Content

This module is designed to display Ez Content. Some Ez Fields can be showed according to configuration.

Clicking on an a content result will redirect to Ez detail page of the given content.

### Module Download

This module is designed to download files. A few field names can be showed according to configuration.

Clicking on this autosuggest result will start a download of the given element. We asume that downloads are meant to be downloaded and not to visit their content detail page.

### Configuration

Please note that the parameter siso\_search.default.autosuggest\_module\_definitions can't be defined multiple times. It would be overridden by the next definition. The module configurations are split only for overview purposes.

The order of the module configurations represent the order of display in the suggestion box.

<table>
<thead>
<tr class="header">
<th>Module</th>
<th>Configuration</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Product</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>siso_search.default.autosuggest_module_definitions:
    product_autosuggest:
        search_limit: 5
        use_prefix_search: false
        min_score: 1
        images_field:
            ez: main_image_url_s
            econtent: ses_product_ses_datamap_ext_main_image_url_value_s
        add_to_basket: true #add to cart
        type_id:
            - 2
        path: &#39;/2/&#39;
        section:
            - 1
        search_fields:
            - ses_product_ses_sku_value_s
            - ses_product_ses_name_value_s
        result_fields:
            - ses_product_ses_sku_value_s
            - ses_product_ses_name_value_s
        result_fields_separator: &#39; - &#39;
        text_limit: 35</code></pre>
</td>
<td><pre><code>search_limit: (int) Defines amount of elements to display.</code></pre>
<pre><code>images_field: (string) defines images path for products depending on the dataprovider</code></pre>
<pre><code>add_to_basket: (bool) defines if add to basket button is shown.</code></pre>
<pre><code>types: (array) additional filter for element types.</code></pre>
<pre><code>path: (string) additional filter for path.</code></pre>
<pre><code>section: (array) additional filter for section.</code></pre>
<pre><code>search_fields: (array) Solr search field names to perform the search.</code></pre>
<pre><code>result_fields: (array) Solr result field names to be shown on results.</code></pre>
<pre><code>result_fields_separator: (string) text to be used as a separator for result fields.</code></pre>
<pre><code>text_limit: (int) limits the length of the resulting string.</code></pre></td>
<pre><code>use_prefix_search: (boolean) if true a ngram fields can be defined in "</code></pre></td>
<pre><code>min_score: is used in case use_prefix_search = false. Only if Solr score is higher than min_score product is returned</code></pre></td>
<pre><code>search_fields", otherwise autosuggest uses a prefix for search (searchterm + *)</code></pre></td>
</tr>
<tr>
<td> Category</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>siso_search.default.autosuggest_module_definitions:
category_autosuggest:
    search_limit: 5
    images: true
    types:
        - 38
    path: &#39;/2/&#39;
    section:
        - 1
    search_fields:
        - ses_category_ses_name_value_s
    result_fields:
        - ses_category_ses_name_value_s
    result_fields_separator: &#39; - &#39;
    text_limit: 60</code></pre>
</td>
<td> 
<pre><code>search_limit: (int) Defines amount of elements to display.</code></pre>
<pre><code>images: (bool) defines if category icon is shown.</code></pre>
<pre><code>types: (array) additional filter for element types.</code></pre>
<pre><code>path: (string) additional filter for path.</code></pre>
<pre><code>section: (array) additional filter for section.</code></pre>
<pre><code>search_fields: (array) Solr search field names to perform the search.</code></pre>
<pre><code>result_fields: (array) Solr result field names to be shown on results.</code></pre>
<pre><code>result_fields_separator: (string) text to be used as a separator for result fields.</code></pre>
<pre><code>text_limit: (int) limits the length of the resulting string.</code></pre></td>
</tr>
<tr>
<td> Content</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>siso_search.default.autosuggest_module_definitions:
content_autosuggest:
    search_limit: 5
    images: true
    section: 
        - 1
    search_fields:
        - article_title_value_s
        - article_intro_value_s
        - article_body_value_s
    result_fields:
        - article_title_value_s
        - article_intro_value_s
        - article_body_value_s
    result_fields_separator: &#39; - &#39;
    text_limit: 60</code></pre>
</td>
<td> 
<pre><code>search_limit: (int) Defines amount of elements to display.</code></pre>
<pre><code>images: (bool) defines if content icon is shown.</code></pre>
<pre><code>section: (array) additional filter for section.</code></pre>
<pre><code>search_fields: (array) Solr search field names to perform the search.</code></pre>
<pre><code>result_fields: (array) Solr result field names to be shown on results.</code></pre>
<pre><code>result_fields_separator: (string) text to be used as a separator for result fields.</code></pre>
<pre><code>text_limit: (int) limits the length of the resulting string.</code></pre></td>
</tr>
<tr>
<td>Download</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>siso_search.default.autosuggest_module_definitions:
download_autosuggest:
    search_limit: 5
    images: true
    search_fields:
        - file_name_value_s
        - file_description_value_s
    result_fields:
        - file_name_value_s
        - file_description_value_s
    mime_types:
        - application/pdf
    result_fields_separator: &#39; - &#39;
    text_limit: 60</code></pre>
</td>
<td><pre><code>search_limit: (int) Defines amount of elements to display.</code></pre>
<pre><code>images: (bool) defines if download icon is shown.</code></pre>
<pre><code>search_fields: (array) Solr search field names to perform the search.</code></pre>
<pre><code>result_fields: (array) Solr result field names to be shown on results.</code></pre>
<pre><code>mime_types: (array) Search only the mime types specified here.</code></pre>
<pre><code>result_fields_separator: (string) text to be used as a separator for result fields.</code></pre>
<pre><code>text_limit: (int) limits the length of the resulting string.</code></pre></td>
</tr>
</tbody>
</table>

## Autosuggest Redirect Controller

This controller redirects to different pages according to passed type. It has been introduced in order to speed up linking to the detail pages without calling expensive methods in order to generate a route/url. 

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td>This is controller that takes care of redirection.</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>class AutosuggestRedirectController extends BaseController
{
}</code></pre>
</td>
</tr>
<tr>
<td><p>Currently we only have 3 redirection types</p>
<p><br />
</p></td>
<td><br />
</td>
</tr>
<tr>
<td>Each redirection type is a service defined in configuration.</td>
<td><div class="content-wrapper">
<p>Example:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>redirect_generator_id: siso_search.autosuggest_redirect_generator.product</code></pre>
</td>
</tr>
<tr>
<td><pre><code>SearchAutosuggestRedirectGeneratorProduct</code></pre>
<p>type:</p>
<pre><code>product_autosuggest</code></pre>
<pre><code></code></pre></td>
<td><div class="content-wrapper">
<p>Will fetch SKU  from $request object and will return a string containing the URL to redirect. The languages are injected into the service from the respective eZ site-access aware parameter.</p>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>/**
 * @param Request $request
 * @return string
 */
public function generateRedirectUrl(Request $request)
{
    $sku = $request-&gt;query-&gt;get(&#39;sku&#39;, &#39;&#39;, false);
    $dataProvider = $this-&gt;catalogProvider-&gt;getDataProvider();
    $element = $dataProvider-&gt;fetchElementBySku(
        $sku,
        array(),
        $this-&gt;languages
    );
    $redirectUrl = $element-&gt;getSeoUrl();

    return $redirectUrl;
}</code></pre>
<p><br />
</p>
<pre><code> </code></pre>
<pre><code> </code></pre>
<p><br />
</p>
</td>
</tr>
<tr>
<td><p>Category Type</p>
<pre><code>SearchAutosuggestRedirectGeneratorCategory</code></pre>
<p><br />
</p></td>
<td><div class="content-wrapper">
<p>In a similar to redirect generator for products, it will return the URL of the category.</p>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$identifier = $request-&gt;query-&gt;get(&#39;identifier&#39;, &#39;&#39;, false);
$dataProvider = $this-&gt;catalogProvider-&gt;getDataProvider();
$element = $dataProvider-&gt;fetchElementByIdentifier(
    $identifier,
    $this-&gt;languages
);
$redirectUrl = $element-&gt;getSeoUrl();

return $redirectUrl;
</code></pre>
</td>
</tr>
<tr>
<td><pre><code>SearchAutosuggestRedirectGeneratorDownload</code></pre>
<p>type:</p>
<pre><code>download_autosuggest</code></pre></td>
<td><div class="content-wrapper">
<p>This is a little bit more complex. It uses Ez API to fetch the media element by identifier and language. Then it creates a download link.</p>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>public function generateRedirectUrl(Request $request)
{
    $contentService = $this-&gt;repository-&gt;getContentService();
    $identifier = $request-&gt;query-&gt;get(&#39;identifier&#39;, &#39;&#39;, false);
    /** @var \eZ\Publish\Core\Repository\Values\Content\Content $ezContent */
    $ezContent = $contentService-&gt;loadContent($identifier, $this-&gt;languages);

    //todo: find out if there is a simpler way to get download content URL.
    $downloadUrl =
        &#39;/content/download/&#39; .
        $ezContent-&gt;id . &#39;/&#39; .
        $ezContent-&gt;getField(&#39;file&#39;)-&gt;id . &#39;/&#39; .
        &#39;version/&#39; .
        $ezContent-&gt;versionInfo-&gt;contentInfo-&gt;currentVersionNo . &#39;/&#39; .
        &#39;file/&#39; .
        $ezContent-&gt;getFieldValue(&#39;file&#39;)-&gt;fileName;

    return $downloadUrl;
}</code></pre>
</td>
</tr>
<tr>
<td><p>All redirect services implements the following interface:</p>
<p><br />
</p></td>
<td><pre><code>AutosuggestRedirectUrlGeneratorInterface</code></pre></td>
</tr>
</tbody>
</table>

## Cookbook

### How to add additional elements to autosuggest module

Any Solr field can be displayed in autosuggest results. So to add additional elements to display in any module is quite simple:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td><ol>
<li>Identify the Solr field you would like to show.</li>
</ol>
<p>For this its convenient to access Solr http interface and search for the required field.</p>
<p>For example we would like to show a preview of PDF content in download module</p>
<p>Content of indexed files are stored in this Solr field:</p>
<pre class="syntax language-json"><code>file_file_file_content_t</code></pre>
<pre class="syntax language-json"><code></code></pre></td>
<td><br />
</td>
</tr>
<tr>
<td> 2. Add Solr field to configuration</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>siso_search.default.autosuggest_module_definitions:
download_autosuggest:
    search_limit: 5
    images: true
    search_fields:
        - file_name_value_s
        - file_description_value_s
    result_fields:
        - file_name_value_s
        - file_description_value_s
+       - file_file_file_content_t
    mime_types:
        - application/pdf
    result_fields_separator: &#39; - &#39;
    text_limit: 60</code></pre>
</td>
</tr>
<tr>
<td><p>3. This will show the pdf content in each result line. But what if you would like to customise this result line?</p>
<p>Each template have the full result line to allow template customisation without service modification.</p>
<p>For example, template for download module is:</p>
<pre><code>Resources/views/Search/autosuggest/search_autosuggest_download_line.html.twig</code></pre></td>
<td><p>Array resultLine will have all information received from Solr, so it can be handy to display different fields any way you want.</p>
<p><br />
</p></td>
</tr>
</tbody>
</table>

#### Example: add a "new" label to certain products

It may also happen that you need to show some information which is not available directly in Solr and you have to process it. For this it is very easy to create an indexer plugin and have your information processed. 

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Task: Display a flag with for new products</td>
<td><br />
</td>
</tr>
<tr>
<td><p>First we need to define what is a "new" product.</p>
<p>Let's say that a new product is a product that was published a month ago.</p>
<p>We could use this Solr field:</p>
<pre class="syntax language-json"><code>&quot;published_dt&quot;: &quot;2016-01-19T09:59:24Z&quot;</code></pre>
<p>to calculate if the product is new or not.</p>
<p><br />
</p></td>
<td><br />
</td>
</tr>
<tr>
<td><p>Create an indexer plugin to add a new boolean field set to true if current date - product published date &lt;= 30 days.</p>
<p>Please refer to this documentation to build indexer plugin:</p>
<p>For Ez Data Provider: <a href="Search---Cookbook_23560665.html#Search-Cookbook-Cookbook-Howtoimplementanindexerpluginforcustomfieldtypes">Search - Cookbook#Cookbook-Howtoimplementanindexerpluginforcustomfieldtypes</a></p>
<p>For Econtent Data Provider: <a href="How-to-implement-a-custom-indexer-plugin-for-econtent_23560600.html">How to implement a custom indexer plugin for econtent</a></p>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p></td>
<td><br />
</td>
</tr>
<tr>
<td><p>Let's assume the new created field is ext_product_new_s</p>
<p>Now we just need to add this field to the result fields of Solr in product configuration.</p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>product_autosuggest:
    search_limit: 10
    images_field: main_image_url_s
    display_cart_info: true #add to cart
    types:
        - 2
    path: &#39;/2/&#39;
    section: 1
    search_fields:
        - ses_product_ses_sku_value_s
        - ses_product_ses_name_value_s
    result_fields:
        - ses_product_ses_sku_value_s
        - ses_product_ses_name_value_s
+       - ext_product_new_s
    result_fields_separator: &#39; - &#39;
    text_limit: 35
</code></pre>
</td>
</tr>
<tr>
<td><p>Now you need to modify the template to read that value:</p>
<p>Product template is:</p>
<pre><code>/Resources/views/Search/autosuggest/search_autosuggest_product_line.html.twig</code></pre>
<p>Please consider that even it is not being used, in every template you have array resultLine with all Solr information.</p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>/* In a normal configuration resultLine array will look like this: */
array (size=5)
  &#39;id&#39; =&gt; string &#39;content9982gerde&#39; (length=16)
  &#39;ses_product_ses_name_value_s&#39; =&gt; string &#39;DMX Followspot HMI-1200&#39; (length=23)
  &#39;ses_product_ses_sku_value_s&#39; =&gt; string &#39;40180&#39; (length=5)
  &#39;main_image_url_s&#39; =&gt; string &#39;/var/ezdemo_site/storage/images/4/4/1/1/531144-2-ger-DE/40180.jpg&#39; (length=65)
  &#39;meta_indexed_language_code_s&#39; =&gt; string &#39;ger-DE&#39; (length=6)</code></pre>
<p>Additionally, if you change the configuration to support your new indexed field, that new field should be part of this array:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>array (size=5)
  &#39;id&#39; =&gt; string &#39;content9982gerde&#39; (length=16)
  &#39;ses_product_ses_name_value_s&#39; =&gt; string &#39;DMX Followspot HMI-1200&#39; (length=23)
  &#39;ses_product_ses_sku_value_s&#39; =&gt; string &#39;40180&#39; (length=5)
  &#39;main_image_url_s&#39; =&gt; string &#39;/var/ezdemo_site/storage/images/4/4/1/1/531144-2-ger-DE/40180.jpg&#39; (length=65)
  &#39;meta_indexed_language_code_s&#39; =&gt; string &#39;ger-DE&#39; (length=6)
+ &#39;ext_product_new_s&#39; =&gt; bool true
 </code></pre>
</td>
</tr>
<tr>
<td>The template modification might look like this:</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;div class=&quot;row&quot;&gt;
    &lt;div class=&quot;small-8 large-8 columns&quot;&gt;
        &lt;a href=&quot;{{ (&#39;/redirect_switcher?sku=&#39; ~ sku ~ &#39;&amp;type=&#39; ~ type)|st_siteaccess_path }}&quot;&gt;
            {% if imageSrc is defined %}
                &lt;img src=&quot;{{ imageSrc }}&quot; width=&quot;50px&quot;/&gt;
            {% endif %}
            {{ set_bold_text(searchQueryString, productText) }}
+           {% if resultLine.ext_product_new_s is defined and resultLine.ext_product_new_s %}
+               &lt;!-- DISPLAY FANCY ANIMATED GIF WITH NEW LABEL --&gt;
+               &lt;img border=&quot;0&quot; alt=&quot;animated gif&quot; src=&quot;new3__e0.gif&quot;&gt;
+           {% endif %}     
        &lt;/a&gt;
    &lt;/div&gt;
        {% if addToBasket %}
&lt;div class=&quot;small-4 large-4 columns&quot;&gt;
&lt;form action=&quot;/basket/add&quot; method=&quot;post&quot;&gt;
    &lt;section class=&quot;js-add-to-basket-parent&quot;&gt;
        &lt;input type=&quot;hidden&quot; name=&quot;ses_basket[{{ number }}][quantity]&quot; value=&quot;1&quot;&gt;
        &lt;input type=&quot;hidden&quot; name=&quot;ses_basket[{{ number }}][sku]&quot; value=&quot;{{ sku }}&quot;&gt;
        &lt;input type=&quot;hidden&quot; name=&quot;autosuggest&quot; value=&quot;autosuggest&quot;&gt;
        &lt;button class=&quot;button tiny js-add-to-basket&quot; style=&quot;float:right;&quot; data-sku=&quot;{{ sku }}&quot; type=&quot;submit&quot; name=&quot;add_to_basket&quot;&gt;
            &lt;i class=&quot;fa fa-cart-plus fa-lg fa-fw&quot;&gt;&lt;/i&gt;
        &lt;/button&gt;
    &lt;/section&gt;
&lt;/form&gt;
&lt;/div&gt;</code></pre>
</td>
</tr>
</tbody>
</table>

### How to override autosuggest services

Autosuggest services are very simple. There are four available services:

    vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Service/SearchAutosuggestAllService.php
    vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Service/SearchAutosuggestCategoryService.php
    vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Service/SearchAutosuggestContentService.php
    vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Service/SearchAutosuggestDownloadService.php

Their structure is very simple:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Search logic is build in method
<pre><code>searchProducts</code></pre>
<p><br />
</p>
<p>Output is generated both at</p>
<pre><code>getDisplayText</code></pre>
<pre><code>generateHtml</code></pre>
<p><br />
Template is rendered in</p>
<pre><code>generateHtml</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;?php
class SearchAutosuggestProductService
{

    /**
     * Main method to fetch products from Solr.
     *
     * @param $searchQueryString
     * @return array
     */
    public function fetchElements($searchQueryString);

    /**
     * This method performs the Solr search with the specified configuration and returns an array with the results
     * 
     * @param $searchQueryString
     * @return array
     */
    protected function searchProducts($searchQueryString);

    /**
     * This method generates the text to be displayed according to current configuration.
     * 
     * @param $resultLine
     * @return string
     */
    protected function getDisplayText($resultLine);

    /**
     * This method returns the rendered template for one result element.
     * 
     * @param $resultLine
     * @param $number
     * @param $searchQueryString
     * @return string
     */
    protected function generateHtml($resultLine, $number, $searchQueryString);
}
</code></pre>
</td>
</tr>
</tbody>
</table>

Also please consider that each autosuggest service implements this interface:

    SearchAutosuggestInterface

And service definition is found in configuration

``` 
Example:
search_service_id: siso_search.autosuggest_service.product
```

In order to create a new autosuggest service, you just need to implement the interface and define the service id in autosuggest configuration.

## Frontend:

### What you should know about template files and autosuggest

Each autosuggest section has its own template. Currently we have 4 templates located in the following directory:

    vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/autosuggest

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Description</th>
<th>Template file</th>
</tr>
</thead>
<tbody>
<tr>
<td><h4 id="SearchAutosuggest-Producttemplate">Product template</h4>
<p>This is the most complex template, since it needs to render image, text, price and add to basket button</p></td>
<td><pre><code>search_autosuggest_product_line.html.twig</code></pre></td>
</tr>
<tr>
<td><h4 id="SearchAutosuggest-Categorytemplate">Category template</h4>
<p>This template renders an Icon (optional) and text.</p></td>
<td><pre><code> search_autosuggest_category_line.html.twig</code></pre></td>
</tr>
<tr>
<td><h4 id="SearchAutosuggest-Contenttemplate">Content template</h4>
<p>This template renders an Icon (optional) and text.</p></td>
<td><pre><code>search_autosuggest_content_line.html.twig</code></pre></td>
</tr>
<tr>
<td><h4 id="SearchAutosuggest-Downloadtemplate">Download template</h4>
<p>This template renders an Icon (optional) and text.</p></td>
<td><pre><code>search_autosuggest_download_line.html.twig</code></pre></td>
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
<th>Description</th>
<th>Details</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<h4 id="SearchAutosuggest-AutosuggestProductTemplateFile">Autosuggest Product Template File</h4>
<p>In this template we are showing the following:</p>
<p>image (if it set)</p>
<p>Autosuggest text (usually SKU - Product Name)</p>
<p>Add to basket button.</p>
<p>To make autosuggest work it is very important to have add to basket button with class: "</p>
<pre><code>js-add-to-basket&quot; This will allow ajax to perform the add product to basket.</code></pre>
<p><br />
</p>
<p>Please note that the object link is generated as a link, and is not using the link functionality of autosuggest js module.</p>
<p>The link goes to autosuggest redirect controller, which will redirect according to type parameter.</p>
<p><br />
</p>
<p><img src="attachments/23560726/23563624.png" class="confluence-embedded-image" height="400" /></p>
</td>
<td><div class="content-wrapper">
<pre><code>search_autosuggest_product_line.html.twig</code></pre>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;div class=&quot;row collapse autocomplete-suggestion-product&quot;&gt;
  {% set columns = 12 %}
  {% if price is defined and price is not empty %}
    {% set columns = columns - 2 %}
  {% endif %}
  {% if addToBasket %}
    {% set columns = columns - 2 %}
  {% endif %}
  &lt;div class=&quot;small-{{ columns }} columns u-padding-right-1x u-line-height-normal&quot;&gt;
    &lt;a href=&quot;{{ (&#39;/redirect_switcher?sku=&#39;~sku~&#39;&amp;type=&#39;~type)|st_siteaccess_path }}&quot;&gt;
      {% if imageSrc is defined %}
        &lt;img src=&quot;{{ imageSrc }}&quot; width=&quot;36px&quot;&gt;
      {% endif %}
      {{ set_bold_text(searchQueryString, productText) }}
    &lt;/a&gt;
  &lt;/div&gt;
  &lt;div class=&quot;small-2 columns text-center&quot;&gt;
    {% if price is defined and price is not empty %}
      {{ price|price_format }}
    {% endif %}
  &lt;/div&gt;
  {% if addToBasket %}
    &lt;div class=&quot;small-2 columns&quot;&gt;
      &lt;form action=&quot;/basket/add&quot; method=&quot;post&quot;&gt;
        &lt;section class=&quot;js-add-to-basket-parent&quot;&gt;
          &lt;input type=&quot;hidden&quot; name=&quot;ses_basket[{{ number }}][quantity]&quot; value=&quot;1&quot;&gt;
          &lt;input type=&quot;hidden&quot; name=&quot;ses_basket[{{ number }}][sku]&quot; value=&quot;{{ sku }}&quot;&gt;
          &lt;input type=&quot;hidden&quot; name=&quot;autosuggest&quot; value=&quot;autosuggest&quot;&gt;
          &lt;input class=&quot;js-autosuggest-product-name&quot; type=&quot;hidden&quot; value=&quot;{{ productText }}&quot;&gt;
          &lt;button class=&quot;button tiny right js-add-to-basket&quot; data-sku=&quot;{{ sku }}&quot; type=&quot;submit&quot; name=&quot;add_to_basket&quot;&gt;
            &lt;i class=&quot;fa fa-cart-plus fa-lg fa-fw&quot;&gt;&lt;/i&gt;
          &lt;/button&gt;
        &lt;/section&gt;
      &lt;/form&gt;
    &lt;/div&gt;
  {% endif %}
&lt;/div&gt;</code></pre>
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<h4 id="SearchAutosuggest-AutosuggestCategoryTemplateFile">Autosuggest Category Template File</h4>
<p>This template only displays an icon for category and autosuggest text.</p>
<p><br />
</p>
<p><img src="attachments/23560726/23563625.png" class="confluence-embedded-image" /></p>
</td>
<td><div class="content-wrapper">
<pre><code>search_autosuggest_category_line.html.twig</code></pre>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;a href=&quot;{{ (&#39;/redirect_switcher?identifier=&#39;~identifier~&#39;&amp;type=&#39;~type)|st_siteaccess_path }}&quot;&gt;
{% if imageIcon is defined %}
    &lt;i class=&quot;fa fa-folder-o&quot; aria-hidden=&quot;true&quot;&gt;&lt;/i&gt;
{% endif %}
{{ set_bold_text(searchQueryString, categoryText) }}
&lt;/a&gt;</code></pre>
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<h4 id="SearchAutosuggest-AutosuggestContentTemplateFile">Autosuggest Content Template File</h4>
<p>This template only displays an icon for content and autosuggest text.</p>
<p><br />
</p>
<p><img src="attachments/23560726/23563626.png" class="confluence-embedded-image" /></p>
</td>
<td><div class="content-wrapper">
<pre><code>search_autosuggest_content_line.html.twig</code></pre>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;a href=&quot;{{ path( &quot;ez_urlalias&quot;, {&quot;locationId&quot;: id}) }}&quot;&gt;
    {% if imageIcon is defined %}
        &lt;i class=&quot;fa fa-file-text-o&quot; aria-hidden=&quot;true&quot;&gt;&lt;/i&gt;
    {% endif %}
{{ set_bold_text(searchQueryString, contentText) }}
&lt;/a&gt;</code></pre>
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<h4 id="SearchAutosuggest-AutosuggestDownloadTemplateFile">Autosuggest Download Template File</h4>
<p>This template only displays an icon for download and autosuggest text. The download link is generated in autosuggest redirect controller.</p>
<p><br />
</p>
<p><img src="attachments/23560726/23563628.png" class="confluence-embedded-image" /></p>
</td>
<td><div class="content-wrapper">
<pre><code>search_autosuggest_download_line.html.twig</code></pre>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;a href=&quot;/redirect_switcher?identifier={{ id }}&amp;language={{ language }}&amp;type={{ type }}&quot;&gt;
{% if imageIcon is defined %}
    &lt;i class=&quot;fa fa-download&quot; aria-hidden=&quot;true&quot;&gt;&lt;/i&gt;
{% endif %}
{{ set_bold_text(searchQueryString, downloadText) }}
&lt;/a&gt;</code></pre>
</td>
<td><br />
</td>
</tr>
<tr>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<h3 id="SearchAutosuggest-TwigFunctiontomarkas&lt;strong&gt;usersearchterms">Twig Function to mark as &lt;strong&gt; user search terms</h3>
<p>This small function is used to insert &lt;strong&gt; tag for user search terms in autosuggest search result.</p>
<p>I.E.:</p>
<p>If user searchs for DMX, then all DMX occurrences will have &lt;strong&gt; tag.</p>
<p>In the following example, the word dap is highlighted using stron tab.</p>
<p><img src="attachments/23560726/23563623.png" class="confluence-embedded-image" /></p>
</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>new \Twig_SimpleFunction(&#39;set_bold_text&#39;,                   array($this, &#39;setBoldText&#39;),
    array(
        &#39;is_safe&#39; =&gt; array(&#39;html&#39;)
    )),</code></pre>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>/**
 * Returns html with added &lt;strong&gt; tag(s) to needle occurences in haystack.
 * Supports multiple words.
 *
 * @param $needle
 * @param $haystack
 * @return mixed
 */
public function setBoldText($needle, $haystack)
{
    // return $haystack if there are strings given, nothing to do
    if (strlen($haystack) &lt; 1 || strlen($needle) &lt; 1) {

        return $haystack;
    }

    preg_match_all(&quot;/$needle+/i&quot;, $haystack, $matches);
    if (is_array($matches[0]) &amp;&amp; count($matches[0]) &gt;= 1) {
        foreach ($matches[0] as $match) {
            $haystack = str_replace($match, &#39;&lt;strong&gt;&#39;.$match.&#39;&lt;/strong&gt;&#39;, $haystack);
        }
    }

    return $haystack;
}</code></pre>
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

Very Important

In all templates we have access to resultLine array, which has all information returned by Solr. You can use this information to adapt the template without overriding the services.

Very important

Autosuggest needs to be generated as fast as possible, so we never have to add additional logic with heavy processing in template or anywhere else. Example: We need to avoid using image transformation functions. If there is the need to have an image of a given size or features it is strongly recommended to do it at index time using an indexer plugin and then use the processed image.

### Advanced Javascript Functionality

The following JS tool is used for autosuggest:

<https://github.com/devbridge/jQuery-Autocomplete>

However to support add to basket we had to do some modifications:

<table>
<thead>
<tr class="header">
<th>Description</th>
<th>Code</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<p>Since we need to keep the autosuggest window opened when user adds to basket suggestions are working as a link and not using onSelect() function from JS Module.</p>
<p>Also a new function was created to hide autosuggest window on any click.</p>

Important: After user clicks on an autosuggest search result bu default search input field is populated with autosuggest result data. This functionality can be turned on and off using parameter:
<pre><code>preserveInput: true</code></pre>
<pre><code>In autocomplete</code></pre>
</td>
<td><div class="content-wrapper">
<p>File: Resources/public/js/app.js</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$(&#39;.js-autocomplete&#39;).autocomplete({
  noCache: true,
  minChars: 2,
  serviceUrl: function () {
    if ($(&#39;body&#39;).attr(&#39;data-siteaccess&#39;) == &#39;/&#39;) {
      return &#39;/search_autosuggest&#39;;
    }
    else {
      return $(&#39;body&#39;).attr(&#39;data-siteaccess&#39;) + &#39;/search_autosuggest&#39;;
    }
  },
  paramName: &#39;search_terms&#39;,
  triggerSelectOnValidInput: false,
  width: Foundation.utils.is_large_up() ? 560 : &#39;&#39;,
  groupBy: &#39;category&#39;,
  maxHeight: 750,
  preserveInput: true, // Enables or disables search input field to be populated with autosuggest result field.
  formatResult: function (suggestion, currValue) {
    return suggestion.data.html;
  }
});

// close autocomplete when user clicks out from the autocomplete window
$(&#39;body&#39;).on(&#39;click&#39;, function() {
  if ($(&#39;.autocomplete-suggestions&#39;).is(&#39;:visible&#39;)) {
    $(&#39;.autocomplete-suggestions&#39;).hide();
  }
});</code></pre>
</td>
</tr>
<tr>
<td><p>Then basket hoplite should had to be modified to leave autosuggest window open if the user clicked add to basket. This was configured in the following file:</p>
<pre><code>Resources/public/js/phalanx/hoplite/basket/basket.js</code></pre>
<p><br />
</p></td>
<td><div class="content-wrapper">
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/phalanx/hoplite/basket/basket.js:102
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>// if the click comes from autosuggest dropdown
if ($(this).parents(&#39;.autocomplete-suggestions&#39;).length) {
  // populate product name in the search field
  // we need to do it this way because autocomplete stays open (see below)
  // because it stays open when we click on a second selection it&#39;s not working
  var productName = $(this).parents(&#39;.js-add-to-basket-parent&#39;).find(&#39;.js-autosuggest-product-name&#39;).val();
  $(&#39;.js-autocomplete&#39;).val(productName);

  // make sure it stays open
  if (Foundation.utils.is_large_up()) {
    $(&#39;.autocomplete-suggestions&#39;).css(&#39;display&#39;, &#39;block&#39;);
  }
}
</code></pre>
</td>
</tr>
</tbody>
</table>

``` 
 
```

## Attachments:

![](images/icons/bullet_blue.gif) [Screen Shot 2016-08-29 at 15.12.29.png](attachments/23560726/23563623.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-08-29 at 15.11.24.png](attachments/23560726/23563624.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-08-29 at 15.11.33.png](attachments/23560726/23563625.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-08-29 at 15.11.56.png](attachments/23560726/23563627.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-08-29 at 15.11.56.png](attachments/23560726/23563626.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-08-29 at 15.12.01.png](attachments/23560726/23563628.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-08-29 at 15.11.38.png](attachments/23560726/23563419.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-9-28 21:7:46.png](attachments/23560726/23563539.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-12-8 22:1:56.png](attachments/23560726/23563785.png) (image/png)  
