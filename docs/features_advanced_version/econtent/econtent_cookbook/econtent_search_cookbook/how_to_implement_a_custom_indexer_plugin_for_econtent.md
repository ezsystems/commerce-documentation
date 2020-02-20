# How to implement a custom indexer plugin for econtent

Creating a plugin for econtent is very easy. In the following example we will create a plugin for econtent that will index an additional field based on a serialised array that comes from the database.

**Input**

- In database we have some keys and values for articles that come in a serialised format like this:

``` 
ses_datamap_additional_data = a:2:{s:4:"Code";s:3:"AXW";s:4:"Name";s:20:"Awesome Speaker AX50";}
```

**Desired Output**

- Index field additional_product_code with value AXW
- Index field additional_product_name with value Awesome Speaker AX50

## What to implement

### Create a plugin class that implements EcontentIndexerPluginInterface

``` php
class AdditionalDataIndexerPlugin implements EcontentIndexerPluginInterface
{
 
    /*
     * It is used as a plugin for econtent indexer.
     *
     */
    public function buildIndexFields(array $econtentData, $elementType)
    {
    }
 
}
```

### Create a service definition for this class.

It is very important to add the tag, otherwise the plugin will not be executed.

```
<parameter key="siso_search.additional_data_indexer_plugin.class">YOURNAMESPACE\AdditionalDataIndexerPlugin</parameter>
  
<service id="siso_search.additional_data_indexer_plugin" class="%siso_indexer.additional_data_indexer_plugin.class%">
    <tag name="siso_search.econtent_indexer_plugin" />
</service>
```

### Put your custom logic

Please note the following.

- We have to loop through the product data map.
- Econtent data map supports 3 types:
- strings which are stored in data_text field
- integers which are stored in data_int field
- floats which are stored in data_float field

For array support we use serialised arrays.

``` php
class AdditionalDataIndexerPlugin implements EcontentIndexerPluginInterface
{
 
    /*
     * It is used as a plugin for econtent indexer.
     *
     */
    public function buildIndexFields(array $econtentData, $elementType)
    {
        // We have to loop for every element in data_map in order to find the desired field name.
        // In this example we are looking for a field named ses_datamap_additional_data
        foreach ($econtentData['data_map'] as $attribute) {
            if ($attribute['attribute_name'] === 'ses_datamap_additional_data ') {
                // We found our field! Now let's get the serialised data!
                // Please note that if the stored attribute is a string the value will be in a field name data_text
                $additionalData = unserialize($attribute['data_text']);
                if (
                    is_array($additionalData) &&
                    isset($additionalData['ArrayFieldHash']['array'][0]['code']) &&
                    isset($additionalData['ArrayFieldHash']['array'][0]['name'])
                ) {
                    return array (
                        'additional_data_code' => $additionalData['ArrayFieldHash']['array'][0]['code'],
                        'additional_data_name' => trim($additionalData['ArrayFieldHash']['array'][0]['name'])
                    );
                }
        }
        return null;
    }
}
``` 
  
### Results

This will create two new Solr fields with the following names and values:

- The prefix (here "ses_product_ses_datamap_ext_") is added automatically and contains the content type (here product) as well
- The suffix _s is added because the indexer detects the value as a string.
But also supports these additional data types with the corresponding suffix:

|Suffix|Data type|
|---|---|
|String|_s|
|Array (Multiple strings)|_ms|
|Float|_f|
|Integer|_i|
|long|_l|

The datatype is specified in EcontentFieldNameResolver

Note about longs:

Please note that Solr max and min integers are the following:

SOLR_MAX_INT = 2147483647;

SOLR_MIN_INT = -2147483648;

```
ses_product_ses_datamap_ext_additional_data_code_s: "AXW"
ses_product_ses_datamap_ext_additional_data_name_s: "Awesome Speaker AX50"
```

If everything works fine, after executing the command line indexer, you should be able to see in Solr this new indexed content.

For instructions about running econtent indexer check this document: [Indexing econtent data](../../econtent_features/indexing_econtent_data/indexing_econtent_data.md).
