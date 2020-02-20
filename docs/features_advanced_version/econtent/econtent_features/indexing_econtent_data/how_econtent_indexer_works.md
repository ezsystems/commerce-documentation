# How Econtent Indexer Works

## Econtent Class Elements

Solr index is build using data from Econtent tables which are:

- sve_class
- sve_class_attributes
- sve_object
- sve_object_attributes
- sve_object_catalog

Since econtent supports n levels of class objects (stored in sve_class), so does the indexer.

In a default configuration we may have only two classes: product and categories, but the model is flexible so it can handle several class types, for example: category_a, sub_category, product, or any other hierarchical schema.

All sve_class elements will have a common name field, and this name will be used by the indexer for having a common Solr field name for all types.

The name is specified in table sve_object_attributes and its identifier is specified in sve_class table. This name will be stored in Solr with this field name: 

`name_s`

## Solr Standard Field Names

This is a description of standard index fields in Solr and its source.

|Solr Field Name|Description|Database Field|Example|
|--- |--- |--- |--- |
|id|id of index element. Is its build concatenating content type + node id + language.||econtent11gerde|
|content_id|Node id of element|sve_object.node_id|11|
|document_type_id|The type of document. For econtent all are 'econtent'||econtent|
|type_id|The id of the class|sve_class.class_id|1|
|type_name_id|The name of the class|sve_class.class_name|product_group|
|section_id|The section is a number that can be used later to separate content.|sve_object.section_id|1|
|meta_indexed_language_code_s|Language code of specified element||ger-DE|
|main_location_id|The node_id of the element.|sve_object.node_id|11|
|main_node_id|The node_id of the element.|sve_object.node_id|11|
|main_location_parent_id|The node_id of the parent element|sve_object.parent_id|2|
|main_location_path_id|The path of the element is the tree node id of all parent elements separated by a slash.|sve_object.path_string|/2/11/|
|main_location_visible_b|A boolean value to determine if the element is visible or not.</br>Please note that the value indexed here is the negated value from DB. Since in DB the field specifies if the element is hidden or not, and in Solr the field specifies if the element is visible or not.|NOT sve_object.hidden|true|
|meta_blocked_b|A boolean value to determine if the element is blocked or not.|sve.object.blocked|false|
|meta_depth_i|The depth of the element. It should be the amount of elements in path_string|sve_object.depth|2|
|name_s|The name of the element as is specified by class id identifier|sve_object_attributes.data_text</br>With attribute id determined in sve_class|DMT|
|meta_modified_dt|The date time value from the database in Solr datetime format|sve_object.change_date|2016-06-21T08:52:40Z|
|main_catalog_segments_ms|Multivalue with all catalog code names|sve_object_catalog.catalog_code|ALL, NORMAL|
|is_main_b|True, if the content_id is the main location id|sve_object.node_id|true|

## Language and Fallback Language

Each indexed language may have a fallback language. In the following example the key of the array is the language and the content the fallback language.

``` yaml
# indexer configuration for econtent
siso_search.default.index_econtent_languages:
    ger-DE: eng-GB #language: ger-DE, fallback: eng-GB
    eng-GB: ger-DE #language: eng-GB, ger-DE
 
```

This means that if a content in language ger-DE have some missing data, then the missing data will be taken from the fallback language data (if there is any).

Econtent languages are defined per attribute, so its possible to have an object with a full set of attributes in one language and a smaller set of attributes in a second language.
