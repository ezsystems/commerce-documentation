#  How Econtent Indexer Works 

## Econtent Class Elements

Solr index is build using data from Econtent tables which are:

  - sve\_class
  - sve\_class\_attributes
  - sve\_object
  - sve\_object\_attributes
  - sve\_object\_catalog

Since econtent supports n levels of class objects (stored in sve\_class), so does the indexer.

In a default configuration we may have only two classes: product and categories, but the model is flexible so it can handle several class types, for example: category\_a, sub\_category, product, or any other hierarchical schema.

All sve\_class elements will have a common name field, and this name will be used by the indexer for having a common Solr field name for all types.

The name is specified in table sve\_object\_attributes and its identifier is specified in sve\_class table. This name will be stored in Solr with this field name: 

    name_s

## Solr Standard Field Names

This is a description of standard index fields in Solr and its source.

<table>
<thead>
<tr class="header">
<th>Solr Field Name</th>
<th>Description</th>
<th>Database Field</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>id</td>
<td><p>id of index element. Is its build concatenating content type + node id + language.</p></td>
<td><br />
</td>
<td><pre class="syntax language-json"><code>econtent11gerde</code></pre></td>
</tr>
<tr>
<td>content_id</td>
<td>Node id of element</td>
<td>sve_object.node_id</td>
<td>11</td>
</tr>
<tr>
<td>document_type_id</td>
<td>The type of document. For econtent all are 'econtent'</td>
<td><br />
</td>
<td>econtent</td>
</tr>
<tr>
<td>type_id</td>
<td>The id of the class</td>
<td>sve_class.class_id</td>
<td>1</td>
</tr>
<tr>
<td>type_name_id</td>
<td>The name of the class</td>
<td>sve_class.class_name</td>
<td><pre class="syntax language-json"><code>product_group</code></pre></td>
</tr>
<tr>
<td>section_id</td>
<td>The section is a number that can be used later to separate content.</td>
<td>sve_object.section_id</td>
<td>1</td>
</tr>
<tr>
<td>meta_indexed_language_code_s</td>
<td>Language code of specified element</td>
<td><br />
</td>
<td>ger-DE</td>
</tr>
<tr>
<td>main_location_id</td>
<td>The node_id of the element.</td>
<td>sve_object.node_id</td>
<td>11</td>
</tr>
<tr>
<td>main_node_id</td>
<td>The node_id of the element.</td>
<td>sve_object.node_id</td>
<td>11</td>
</tr>
<tr>
<td>main_location_parent_id</td>
<td>The node_id of the parent element</td>
<td>sve_object.parent_id</td>
<td>2</td>
</tr>
<tr>
<td>main_location_path_id</td>
<td>The path of the element is the tree node id of all parent elements separated by a slash.</td>
<td>sve_object.path_string</td>
<td>/2/11/</td>
</tr>
<tr>
<td>main_location_visible_b</td>
<td><p>A boolean value to determine if the element is visible or not.</p>
<blockquote>
<p>Please note that the value indexed here is the negated value from DB. Since in DB the field specifies if the element is hidden or not, and in Solr the field specifies if the element is visible or not.</p>
</blockquote></td>
<td>NOT sve_object.hidden</td>
<td>true</td>
</tr>
<tr>
<td>meta_blocked_b</td>
<td>A boolean value to determine if the element is blocked or not.</td>
<td>sve.object.blocked</td>
<td>false</td>
</tr>
<tr>
<td>meta_depth_i</td>
<td>The depth of the element. It should be the amount of elements in path_string</td>
<td>sve_object.depth</td>
<td>2</td>
</tr>
<tr>
<td>name_s</td>
<td><p>The name of the element as is specified by class id identifier</p>
<p><br />
</p></td>
<td>sve_object_attributes.data_text<br />
With attribute id determined in sve_class</td>
<td><pre class="syntax language-json"><code>DMT</code></pre></td>
</tr>
<tr>
<td>meta_modified_dt</td>
<td>The date time value from the database in Solr datetime format</td>
<td>sve_object.change_date</td>
<td><pre class="syntax language-json"><code>2016-06-21T08:52:40Z</code></pre></td>
</tr>
<tr>
<td>main_catalog_segments_ms</td>
<td>Multivalue with all catalog code names</td>
<td>sve_object_catalog.catalog_code</td>
<td>ALL, NORMAL</td>
</tr>
<tr>
<td><pre><code>is_main_b</code></pre></td>
<td>True, if the content_id is the main location id</td>
<td>sve_object.node_id</td>
<td>true</td>
</tr>
</tbody>
</table>

## Language and Fallback Language

Each indexed language may have a fallback language. In the following example the key of the array is the language and the content the fallback language.

``` 
# indexer configuration for econtent
siso_search.default.index_econtent_languages:
    ger-DE: eng-GB #language: ger-DE, fallback: eng-GB
    eng-GB: ger-DE #language: eng-GB, ger-DE
 
```

This means that if a content in language ger-DE have some missing data, then the missing data will be taken from the fallback language data (if there is any).

Econtent languages are defined per attribute, so its possible to have an object with a full set of attributes in one language and a smaller set of attributes in a second language.
