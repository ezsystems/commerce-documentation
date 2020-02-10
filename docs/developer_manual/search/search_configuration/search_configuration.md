#  Search - Configuration 

## General search parameters

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 66%" />
</colgroup>
<thead>
<tr class="header">
<th>Config file ez_search.yml (there is also econtent_search.yml with different values and a general search.yml)</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
 # position of facet block in search. Possible values: left, center
 siso_search.default.facet_position: center</code></pre>
</td>
<td><div class="content-wrapper">
<p>This is to specify if the facet block is on the center or to the left.</p>
<p><img src="attachments/23560662/23563830.png" class="confluence-embedded-image" width="800" /></p>
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.groups.product_list:
    product_list:
        types:
            - ses_product
        path: &#39;/1/2/&#39;
        section:
            - 1
        visibility: true
</code></pre>
</td>
<td><p>These parameters define the types that we search in the product listing section of the website. <strong>All filters from the search group section below are valid here, as well.</strong></p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>    siso_search.default.groups.search:
        product:
            types:
                - ses_product
            path: &#39;/1/2/&#39;
            section:
                - 1
            visibility: true
        content:
            types:
                - st_module
                - folder
                - article
                - landing_page
                - blog_post
                - event
            path: &#39;/1/2/&#39;
            section:
                - 1
            visibility: true
        files:
            types:
                - file
                - video
            path: &#39;/1/43/&#39;
            section:
                - 3
            visibility: true</code></pre>
</td>
<td><p>These parameters define the tabs and the content of each tab for search section of the website.</p>
<p>In current example we have 3 tabs:</p>
<ol>
<li>Product</li>
<li>Content</li>
<li>Files</li>
</ol>
<p>The label of the tabs are defined in the translation file. messages.de.php and messages.en.php using the key value as id.<br />
In this example the key values are: product, content and files.</p>
<p>Inside each tab there are additional definitions:</p>
<ul>
<li><strong>types</strong>: specify the content types that will be searched for (can be multiple)</li>
<li><strong>path</strong>: specify the ez path of the content.</li>
<li><strong>section</strong>: this additional parameter defines the possible sections of the content. In our configuration products and standard content are in section 1. Files are in section 3. The sections are defined inside ez backend. If multiple sections are defined, products or content must be assigned to either of them (OR operator).</li>
<li><strong>visibility</strong>: this parameters allow to search for content that is/isn't visible.</li>
</ul></td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.product_groups: [&#39;product&#39;, &#39;product_list&#39;]</code></pre>
</td>
<td>Defines the groups that will be used for the product search.</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.query.main_location_only: true</code></pre>
</td>
<td>Defines that search is returning main nodes only.</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.groups.search.preferred: product</code></pre>
</td>
<td><p>Defines which group will be preferred for search as a default choice.</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.limits:
 left:
    3: 3
    6: 6
    12: 12
    24: 24
    48: 48
 center:
     4: 4
     8: 8
     16: 16
     32: 32
     64: 64</code></pre>
</td>
<td><div class="content-wrapper">
<p>This parameter defines elements per page and will populate page size drop down.</p>
<p><img src="attachments/23560662/23563843.png" class="confluence-embedded-image confluence-thumbnail" height="150" /></p>
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.preferred_limit:
    left: 6
    center: 8</code></pre>
</td>
<td>Defines the default limit per design position for the search.</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.sort:
    product_list:
        - score
        - name|asc
        - name|desc
        - sku|asc
        - sku|desc
        - price|asc
        - price|desc
    product:
        - score
        - name|asc
        - name|desc
        - sku|asc
        - sku|desc
        - price|asc
        - price|desc
    content:
        - score
        - name|asc
        - name|desc
    files:
        - score
        - name|asc
        - name|desc</code></pre>
</td>
<td>These parameters define the sorting options for each particular group.</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.sort.preferred:
    product_list: score
    product: score
    content: score
    files: score</code></pre>
</td>
<td><p>Defines default sorting per group. </p>
<p>Please note that score is a Solr field which is a numeric value that can affect the relevance of the elements. More info on Solr score can be found here: <a href="https://wiki.apache.org/solr/SolrRelevancyFAQ" class="external-link">https://wiki.apache.org/solr/SolrRelevancyFAQ</a></p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code># eZ Content
siso_search.default.field_boosts:
    product:
        ses_name: 50
        ses_sku: 100
        ses_intro: 20
        ses_short_description: 2
        ses_long_description: 1
        ses_manufacturer: 2

# Econtent
siso_search.default.field_boosts:
    product:
        text: 1
        ses_product_ses_sku_value_s: 100
        ses_product_ean_value_s: 100
        ses_product_ses_name_value_t: 50
        ses_product_sub_headline_value_t: 1
        ses_product_long_description_value_t: 20
        ses_product_ses_datamap_product_group_names_value_t: 1
        ses_product_ses_datamap_category_value_t: 2</code></pre>
</td>
<td><div class="content-wrapper">
<p>Configuration for boosting</p>
<p>The boosting influences the calculation of the relevancy. Using boosting factors it can be "controlled" which fields are more important / more relevant for the search terms than other fields. A higher value will boost a hit, which means if the search engine find a search term a field boosted with a high factor the matching document will have a better relevancy. Please mind that the existence of search terms is crucial for the calculation of relevancy. The field boost has no influence on the order of results, if a search was queried without terms (e.g. a plane product list in the catalog).</p>
<p>By default the search engine uses a factor of 1. A boosting of values below 1 (e.g. 0.5) will reduce the relevance of this field.</p>
<p>Currently, the implementation of the field boosing API is not consistent. That means, that the way how it is configured differs for Econtent and eZ Content.</p>
<p><strong>eZ Content:</strong></p>
<p>The configured field name values are field identifiers of their respective ContentTypes (e.g. field ses_name of type ses_product ). Fields added by an indexer plugin can currently not be boosted.</p>
<p><strong>Econtent:</strong></p>
<p>The field name values are raw Solr field names. Thus, all indexed fields can be taken into consideration for boosting, but must also be adapted in the configuration always when they change in the index. Furthermore, you SHOULD specify the default search field 'text' here, as it would not be queried otherwise. Boosts in Econtent will activate the qf parameter, which overrides the default field (df parameter).</p>

<p>If you specify boosting fields in Econtent, the shop will only search in these fields!</p>
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code># ********* configuration for facets ********* #
#facet definitions
siso_search.default.simple_facet_definitions:
    price_range:
        #possible values: single|multi_and|multi_or
        filter_type: &#39;single&#39;
        #possible values: alpha|numeric|manual|range
        sort_type: &#39;range&#39;
        #sort_type_order: &#39;XS|S|M|L|XL|XXL&#39;
        index_field: &#39;ext_ses_price_range_ms&#39;
        # ONLY VALID FOR THIS FACET
        index_range_boundries: 0|10|20|50|100
    manufacturer:
        filter_type: &#39;multi_or&#39;
        sort_type: &#39;alpha&#39;
        index_field: &#39;ses_product_ses_manufacturer_value_s&#39;
    weight:
        filter_type: &#39;multi_or&#39;
        sort_type: &#39;numeric&#39;
        index_field: &#39;ext_ses_spec_weight_f&#39;
    dimensions:
        filter_type: &#39;multi_or&#39;
        sort_type: &#39;alpha&#39;
        index_field: &#39;ext_ses_spec_dimensions_s&#39;
    category:
        filter_type: &#39;multi_or&#39;
        sort_type: &#39;alpha&#39;
        index_field: &#39;location_parent_id_mid&#39;
        # ONLY VALID FOR THIS FACET
        location_id_values: true
    impedance:
        filter_type: &#39;single&#39;
        sort_type: &#39;numeric&#39;
        index_field: &#39;ext_ses_spec_impedance_s&#39;
    availability:
        filter_type: &#39;single&#39;
        sort_type: &#39;alpha&#39;
        index_field: &#39;ses_product_ses_stock_numeric_value_s&#39;
    discontinued:
        filter_type: &#39;single&#39;
        sort_type: &#39;alpha&#39;
        index_field: &#39;ses_product_ses_discontinued_value_s&#39;
</code></pre>
</td>
<td><div class="content-wrapper">
<p>This part defines the facet configuration.</p>
<p>Each facet can have the following configuration:</p>
<ul>
<li><strong>key</strong>: it is used to identify each facet. (I.E.: price_range, manufacturer, etc).</li>
<li><p><strong>filer_type</strong>: The possible values are<br />
single|multi_and|multi_or.<br />
Single: the user may select only one facet element.<br />
Multi Or: the user may select multiple facet elements and each of them use OR as operator. (The results can match one facet OR the other)<br />
Multi And: the user may select multiple facet elements and each of them use AND as operator. (The results should match all selected facets, for example if products have connectors and the results should have HDMI and USB-3)</p>

<p>Multi And has currently an interface issue. If a multiple selection of this kind of facet leads to an empty result page, the only possible way for the user to get back is to click the 'remove all facets' button. This can be inconvenient if a lot of facets exists and have already been chosen. Multi And facets are a rare requirement, though.</p>

</li>
<li><strong>sort_type</strong>: It defines how the facet box should be sorted. Some time is not enough to have only numeric or alphanumeric sorting.</li>
<li><strong>sort_type_order</strong>: For example for clothes size it is nice to have S | M | L | XL. </li>
<li><strong>index_field</strong>: name of corresponding field in solr</li>
<li><strong>index_range_boundries</strong>: currently is only available for Price Range facet. It defines the ranges of current values.</li>
</ul>
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.used_facets:
    product_list:
        common: [&#39;manufacturer&#39;, &#39;category&#39;, &#39;price_range&#39;, &#39;availability&#39;, &#39;discontinued&#39;]
        specifications: [&#39;weight&#39;, &#39;dimensions&#39;, &#39;impedance&#39;]
    product:
        common: [&#39;manufacturer&#39;, &#39;category&#39;, &#39;price_range&#39;]
        specifications: [&#39;weight&#39;, &#39;dimensions&#39;, &#39;impedance&#39;]</code></pre>
</td>
<td>This section describes which facets will be displayed for each group.</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.search.auto_suggest_limit: 10
siso_search.default.search.auto_suggest_fields:
    - ses_product_ses_sku_value_s
    - ses_product_ses_name_value_s
    - ses_variant_list_s
    - ses_variant_codes_ms
    - ses_variant_sku_ms
    - ses_variant_desc_ms</code></pre>
</td>
<td><p>These are autosuggest parameters only for quick order. User input in quick order will search this defined Solr fields.</p>
<p>Also the limit can be specified here.</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>#optional configuration for indexing the specification fields
siso_search.default.search_mapping_specification:
     manufacturer:
         alias: [&#39;Manufacturer&#39;, &#39;Hersteller&#39;, &#39;Peak&#39;]
         type: string
     weight:
         alias: [&#39;Weight&#39;, &#39;Netto-Gewicht&#39;]
         type: float
     dimensions:
         alias: [&#39;Dimensions (LxWxH)&#39;, &#39;Maße (BxHxT)&#39;]
         type: string
</code></pre>
</td>
<td><div class="content-wrapper">

<p>This configuration is deprecated (MatrixFieldType does not exists anymore in eZ Platform)</p>
<p><br />
</p>
<p>These additional configuration allows some predefined rules for product specification matrix index.</p>
<p>alias</p>
<p>This is useful to specify additional words for a given specification matrix parameter. The indexer will index only the first element of the alias array.</p>
<p>type: it allows to specify a data type for the given specification matrix element. I.E. if float is set, the indexer will do a string to float conversion of the value.</p>
<p>separator: if this parameter is set, the indexer will create a Solr field with multiple elements using the specified separator to explode the string into an array.</p>
<p>I.E. for separator:</p>
<p>Specification Matrix Field Name: Dimensions<br />
Specification Matrix Field Value: 220 x 110 x 150</p>
<p>If we specify 'x' as separator, the indexer will create a Solr field with multiple elements that will look like this: {220, 110, 150} </p>
<p><br />
</p>
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_search.default.category_cache:
    rootNodeId: 62
    maxProductCategoryDepth: 99
    productCategoryLimit: 999
    add_parent_category_name: true
    parent_category_name_separator: &#39; / &#39;</code></pre>
</td>
<td><p>As category names are currently not indexed, category names are stored in a cache called stash.</p>
<p>This parameter defines the following:</p>
<p>rootNodeId: It should be set with the node of e-shop product main category.</p>
<p>maxProductCategoryDepth: it defines how many levels of subcategories should be set in this cache.</p>
<p>productCategoryLimit: it defines the maximum category names that will be stores in this cache.</p>
<p>add_parent_category_name: If set to tru it will also index the parent category name.</p>
<p>parent_category_name_separator: this is string separator to concatenate parent category and current category.</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>globals:
    boxes_visibility_limit: 10
    items_visibility_limit: 5
    mega_visibility_limit: 50</code></pre>
</td>
<td>
<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><pre><code>boxes_visibility_limit</code></pre></td>
<td>If more than limit the boxes are hidden by default and the toggler is enabled in order to show/hide more items</td>
</tr>
<tr>
<td><pre><code>items_visibility_limit</code></pre></td>
<td>If more than limit the search filter is visible inside a single facet box to help find faster what you need</td>
</tr>
<tr>
<td><pre><code>mega_visibility_limit</code></pre></td>
<td>If more than this limit we use mega dropdown with columns to make the user experience better.</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code># Configuration to escape or remove Solr special characters from search query.
# Possible values: escape, remove or empty.
siso_search.default.solr_special_characters: escape</code></pre>
</td>
<td><div class="content-wrapper">
 
<ul>
<li><strong>escape</strong>: will escape every Solr special character from the user search query.</li>
<li><strong>remove</strong>: will remove every Solr special character from the user search query.</li>
<li><strong>~</strong>: will keep all Solr special characters.</li>
</ul>
 
<p>These are the current Solr special characters:</p>
<pre class="" data-syntaxhighlighter-params="brush: text; gutter: false; theme: Confluence" data-theme="Confluence"><code>+ - &amp;&amp; || ! ( ) { } [ ] ^ &quot; ~ * ? : \</code></pre>
<p>Every character listed above has a special function and required syntax in Solr if they are not escaped.</p>

<p>Here are some examples:</p>
<p>'-' not operator, when preceding a word token. <br />
<br />
'+' operator, when preceding a word token implies an AND operator. The term after the + symbol definitely exists somewhere in the documents searched. <br />
<br />
'*' wildcard operator when followed by or preceded by a word token. Use wildcards to look for spelling variations and alternate word endings. <br />
<br />
() characters serve to group tokens with AND/OR operator. Search terms within parentheses are read first, then terms outside parentheses is read next. <br />
<br />
" operator surrounding word tokens will cause the word tokens to be treated as is and as a phrase.</p>
<p><br />
</p>
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>siso_search.default.query.main_location_only: true</code></pre>
</td>
<td>If true the search and autosuggest will only return one hit, if the product exists in several locations</td>
</tr>
</tbody>
</table>

## Econtent search parameters

<table>
<thead>
<tr class="header">
<th>Config file econtent.yml</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    silver_econtent.default.section_filter: enabled
    silver_econtent.default.catalog_filter_default_catalogcode:
        - ALL
        - NORMAL</code></pre>
</td>
<td><div class="content-wrapper">

<p>These parameters are not only dedicated to search. But they are used in search result filtering, as well, if econtent is configured as data provider.</p>
<p>If <em>$section_filter;silver_econtent$</em> is enabled, then the product/catalog search result list is restricted to elements which are assigned to catalog codes, provided by <em>$catalog_filter_default_catalogcode;silver_econtent$</em>.</p>
</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [Screen Shot 2016-02-05 at 16.17.03.png](attachments/23560662/23563829.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-02-05 at 16.18.31.png](attachments/23560662/23563830.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-02-05 at 16.23.25.png](attachments/23560662/23563843.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2016-02-05 at 16.29.22.png](attachments/23560662/23563844.png) (image/png)  
