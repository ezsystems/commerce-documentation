# econtent - Configuration 

### Important configuration for projects

The indexer for econtent needs to know which languages are used in your project and if you are using a fallback language.

Please adapt this setting according to your project needs:

``` 
# indexer configuration for econtent
siso_search.default.index_econtent_languages:
    ger-DE: eng-GB #language: ger-DE, fallback: eng-GB
    eng-GB: ger-DE #language: eng-GB, ger-DE
```

### Econtent Configuration

This is econtent.yml file explained 

<table>
<thead>
<tr class="header">
<th>parameters</th>
<th>comments</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code> #possible values: econtent or ez5
 silver_eshop.default.catalog_data_provider: econtent
</code></pre></td>
<td>This will configure the shop to use econtent as data provider.</td>
</tr>
<tr>
<td><pre><code>#definition for econtent languages
silver_econtent.default.languages: [ eng-GB, ger-DE]</code></pre></td>
<td><p>This languages should are used for econtent. If there are other languages present in database they will be ignored.</p></td>
</tr>
<tr>
<td><pre><code>#language for engl site access with no fallback.
silver_econtent.engl.languages: [ eng-GB ]</code></pre></td>
<td>This is the language definition for the English site access.</td>
</tr>
<tr>
<td><pre><code>#language for ger site access with english as fallback.
silver_econtent.ger.languages: [ ger-DE, eng-GB ]</code></pre></td>
<td><p>This is the language definitions for the German site access. Please note that the second language specified here is the fallback language, this means that if a content is not found in the first language it will use the second language instead.</p>
<p>I.E.: There is a product which is defined in ger-DE, it has a name in ger-DE, but the description is only in eng-GB. With this configuration, the product will show and will be indexed as a ger-DE with eng-GB description.</p>
<p><br />
</p></td>
</tr>
<tr>
<td><pre><code>silver_econtent.default.table_object: sve_object
silver_econtent.import.table_object: sve_object_tmp
silver_econtent.default.table_object_attributes: sve_object_attributes
silver_econtent.import.table_object_attributes: sve_object_attributes_tmp
silver_econtent.default.table_class: sve_class
silver_econtent.default.table_class_attributes: sve_class_attributes
silver_econtent.default.table_externaldata: ses_externaldata</code></pre></td>
<td>These are the table names definition.</td>
</tr>
<tr>
<td><pre><code> </code></pre>
<pre><code></code></pre>
<pre><code>silver_econtent.default.section_filter: disabled</code></pre>
<pre><code>silver_econtent.default.table_catalog_filter: sve_object_catalog
silver_econtent.default.filter_SQL_where:
silver_econtent.default.catalog_filter_default_catalogcode:</code></pre></td>
<td><p>Used for catalog segmentation</p>
<p>enabled or disabled</p>
<p>Defines the name of the table to be used<br />
The where condition in order to  limit product catalog<br />
A catalog segmentation code used for this siteaccess or by default </p></td>
</tr>
<tr>
<td><pre><code>silver_econtent.default.root_node_id: 2</code></pre></td>
<td><div class="content-wrapper">
<p>This is the root node id definition. Make sure your root node in save_object table has this id.</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>Example:
 
node_id class_id    parent_id   change_date blocked hidden  priority    section url_alias   path_string depth   main_node_id
2   1   0   NULL    0   0   1   1   Highlite    /2/ 1   2</code></pre>
<p><br />
</p>
<p><br />
</p>
</td>
</tr>
<tr>
<td><pre><code>#navigation configuration
silver_eshop.default.econtent_catalog_data_provider.filter:
 navigation:
 contentTypes: 1
        limit: 20</code></pre></td>
<td><p>This will configure navigation.</p>
<p>contentTypes is the id specified in sve_class. In our examples 1 is for product groups and 2 is for products.</p>
<p>The limit is the amount to show in navigation service.</p></td>
</tr>
<tr>
<td><pre><code>#configuration for the factory
silver_eshop.default.econtent_catalog_factory.product_group: createCatalogNode
silver_eshop.default.econtent_catalog_factory.product: createOrderableProductNode</code></pre></td>
<td>These are the definition for the methods that actually create a catalog node.</td>
</tr>
<tr>
<td><pre><code>silver_econtent.default.sku_id: 202</code></pre></td>
<td><div class="content-wrapper">
<p>This is the definition for the field that will be used for SKU. This is required for building the catalog element. This is linked to sve_class_attributes.attribute_id</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>attribute_id   class_id    attribute_name  ezdatatype  sort_field
202 2   ses_sku ezstring    data_text</code></pre>
<p><br />
</p>
<p><br />
</p>
</td>
</tr>
<tr>
<td><pre><code>silver_econtent.default.class_id_catalog: 1</code></pre></td>
<td><div class="content-wrapper">
<p>This will specify the id of product group elements found in se_class.class_id</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>class_id   class_name  name_identifier
1   product_group   101</code></pre>
<p><br />
</p>
<p><br />
</p>
</td>
</tr>
<tr>
<td><pre><code>silver_econtent.default.mapping.product_group:
 identifier:
 id: &quot;node_id&quot;
 extract: false
    parentElementIdentifier:
 id: &quot;parent_id&quot;
 extract: false
    url:
 id: &quot;url_alias&quot;
 extract: false
    name:
 id: &quot;ses_name&quot;
 extract: &quot;extractText&quot;
 text:
 id: &quot;text&quot;
 extract: &quot;extractText&quot;
 image:
 id: &quot;image&quot;
 extract: &quot;extractImage&quot;</code></pre></td>
<td><p>This defines how content is extracted from product groups.</p>
<p><br />
</p>
<p>There is a complete explanation here: LINK</p>
<p><br />
</p></td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>silver_econtent.default.mapping.product:
    identifier:
        id: &quot;node_id&quot;
        extract: false
    parentElementIdentifier:
        id: &quot;parent_id&quot;
        extract: false
    url:
        id: &quot;url_alias&quot;
        extract: false
    sku:
        id: &quot;ses_sku&quot;
        extract: &quot;extractText&quot;
    manufacturerSku:
        id: &quot;ses_manufacturer_sku&quot;
        extract: &quot;extractText&quot;
    name:
        id: &quot;ses_name&quot;
        extract: &quot;extractText&quot;
    ean:
        id: &quot;ses_ean&quot;
        extract: &quot;extractText&quot;
    text:
        id: &quot;ses_subtitle&quot;
        extract: &quot;extractText&quot;
    subtitle:
        id: &quot;ses_subtitle&quot;
        extract: &quot;extractText&quot;
    image:
        id: &quot;ses_image_main&quot;
        extract: &quot;extractImage&quot;
    shortDescription:
        id: &quot;ses_short_description&quot;
        extract: &quot;extractTextBlock&quot;
    longDescription:
        id: &quot;ses_long_description&quot;
        extract: &quot;extractTextBlock&quot;
    price:
        id: &quot;ses_unit_price&quot;
        extract: &quot;extractPrice&quot;
    cacheIdentifier:
        id: &quot;ses_sku&quot;
        extract: &quot;extractCacheIdentifier&quot;</code></pre>
</td>
<td><div class="content-wrapper">
<p>This defines how content is extracted from products.</p>
<p>extract specifies the method used to extract the value. If it is false the value will be extracted directly from db.</p>
<p><br />
</p>
<p><strong>Image</strong>:</p>
<p>The image path to the product image should be relative to the path "web/var/assets/product_images" (the patch is stored in the config silver_eshop.default.catalog_factory.assets)</p>
<p><strong>Specification data:</strong></p>
<p>Specification data has to be stored in a json format (type ezstring) example (one group "technic" is used).</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;technic&quot;: [
    {
      &quot;label&quot;: &quot;Größe&quot;,
      &quot;value&quot;: &quot;1,69 cm (0.667 Zoll)&quot;
    },
    {
      &quot;label&quot;: &quot;Kompatibilität&quot;,
      &quot;value&quot;: &quot;Epson Stylus Pro 9600, &quot;
    }
  ]
}</code></pre>
<p><br />
</p>
<p><strong>Important</strong>: current the identifier in econtent has to be named "ses_specification"</p>
<p>The attributes of the specification data will be indexed in Solr as well.</p>
<pre><code></code></pre>
<p><br />
</p>
</td>
</tr>
<tr>
<td><pre><code># Definition for prefix ez fields stored in econtent</code></pre>
<pre><code>silver_econtent.default.ez_datatype_attribute_prefix: ezdata_</code></pre></td>
<td><p>This definition is used by this plugin: <a href="#" class="unresolved">SisoNavEcontentImporterPlugin</a></p>
<p>It defines the prefix of the field name that Ez Fields will have in econtent.</p></td>
</tr>
</tbody>
</table>

## Mapping of econtent fields  

The factory maps content to attributes. It uses information from configuration file.

Configuration for mapping Econtent to fields was added to make Econtent Factory more flexibile. The mapping field (ex. Identifier) has to values inside:

1.  **id** - which is the name of the attribute in Econtent (eg. node\_id) or index in the externalData table (eg. typ.0.long\_description)
2.  **extract** - tells if data needs to be extracted. Possible values:  
      - ``` 
        false - the data is not extracted (just set raw from database)
        ```
    
      - ``` 
        method name - data is extracted by the proper factory
        ```
3.  **object** - tell which object need to be created for return eg. TextBlockField

 Ex. it maps "**node\_id**" from content to "**identifier**" and does not extract it (just take raw from datamap)

<table>
<thead>
<tr class="header">
<th>parameters</th>
<th>comment</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>silver_econtent.default.mapping.product:
    identifier:
        id: &quot;node_id&quot;
        extract: false
    parentElementIdentifier:
        id: &quot;parent_id&quot;
        extract: false
    url:
        id: &quot;url_alias&quot;
        extract: false
    sku:
        id: &quot;ses_sku&quot;
        extract: &quot;extractText&quot;
    manufacturerSku:
        id: &quot;ses_manufacturer_sku&quot;
        extract: &quot;extractText&quot;
    name:
        id: &quot;ses_name&quot;
        extract: &quot;extractText&quot;
    ean:
        id: &quot;ses_ean&quot;
        extract: &quot;extractText&quot;
    text:
        id: &quot;ses_subtitle&quot;
        extract: &quot;extractText&quot;
    subtitle:
        id: &quot;ses_subtitle&quot;
        extract: &quot;extractText&quot;
    image:
        id: &quot;ses_image_main&quot;
        extract: &quot;extractImage&quot;
    shortDescription:
        id: &quot;ses_short_description&quot;
        extract: &quot;extractTextBlock&quot;
    longDescription:
        id: &quot;ses_long_description&quot;
        extract: &quot;extractTextBlock&quot;
    price:
        id: &quot;ses_unit_price&quot;
        extract: &quot;extractPrice&quot;
    cacheIdentifier:
        id: &quot;ses_sku&quot;
        extract: &quot;extractCacheIdentifier&quot;</code></pre>
</td>
<td><div class="content-wrapper">
<p>This defines how content is extracted from products.</p>
<p>extract specifies the method used to extract the value. If it is false the value will be extracted directly from db.</p>
<p><br />
</p>
<p><strong>Image</strong>:</p>
<p>The image path to the product image should be relative to the path "web/var/assets/product_images" (the patch is stored in the config silver_eshop.default.catalog_factory.assets)</p>
<p><strong>Specification data:</strong></p>
<p>Specification data has to be stored in a json format (type ezstring) example (one group "technic" is used).</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
  &quot;technic&quot;: [
    {
      &quot;label&quot;: &quot;Größe&quot;,
      &quot;value&quot;: &quot;1,69 cm (0.667 Zoll)&quot;
    },
    {
      &quot;label&quot;: &quot;Kompatibilität&quot;,
      &quot;value&quot;: &quot;Epson Stylus Pro 9600, &quot;
    }
  ]
}</code></pre>
<p><br />
</p>
<p><strong>Important</strong>: current the identifier in econtent has to be named "ses_specification"</p>
<p>The attributes of the specification data will be indexed in Solr as well.</p>
<pre><code></code></pre>
</td>
</tr>
</tbody>
</table>

## Data from an external data source (DB table **ses\_externaldata)**

**EcontentCatalogFactory** for every CatalogElement collects all information from "**ses\_externaldata**" table for given sku. The sku is taken from "data\_text" column in "sve\_object\_attributes" table. It collects different data like pim, sap and typ codes. The it matches the external data with catalog element attributes and also puts all data in dataMap.

Configuration for **externalDataTypes**. It is used for searching different content in externalData table.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><p><code class="sourceCode php">silver_econtent.</code><code class="sourceCode php"><span class="kw">default</code><code class="sourceCode php">.externalDataType:</code><br />
<code class="sourceCode php">    </code><code class="sourceCode php">pim:</code><br />
<code class="sourceCode php">        </code><code class="sourceCode php">identifier: pim</code><br />
<code class="sourceCode php">    </code><code class="sourceCode php">sap:</code><br />
<code class="sourceCode php">        </code><code class="sourceCode php">identifier: sap</code><br />
<code class="sourceCode php">    </code><code class="sourceCode php">typ:</code><br />
<code class="sourceCode php">        </code><code class="sourceCode php">identifier: Typ1</code><br />
<code class="sourceCode php">        </code><code class="sourceCode php">prefix: <span class="kw">TY_</code></p></td>
</tr>
</tbody>
</table>
