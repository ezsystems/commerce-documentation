# Indexing econtent data

If eZ Commerce Advanced is configured to use econtent as data provider the following things have to be taken into consideration when indexing.

## Default indexing

Econtent data can be indexed using the following command:

``` bash
php bin/console silversolutions:indexecontent
```

Econtent Solr configuration uses two cores in order to keep search services available while indexing. This means that the indexer will index econtent data in a back core and then the indexer can be executed again with a swap parameters to swap the cores.

|Indexer options|Results|
|--- |--- |
|php -d memory_limit=-1 bin/console silversolutions:indexecontent -c 5000 -r --siteaccess=import|-c specifies the chunk size, this is how many elements are indexed at a same time.</br>-r option delete all previous econtent data.</br>Important: the parameter "siteaccess=import" will inform the indexer to take the product data from the tmp tables.|
|php -d memory_limit=-1 bin/console silversolutions:indexecontent --live-core|This will index directly on the live core (and not on back core).</br> Please note that if you use -r and --live-core the delete operation will take place before starting the indexer, so the full indexed content will not be available while indexing.|
|php -d memory_limit=-1 bin/console silversolutions:indexecontent swap|This will not index, but swap the cores instead.|


### Additional parameters

There are some parameters that can be specified in the configuration file. 

|||
|--- |--- |
|siso_search.default.index_econtent_languages: [ ger-DE, eng-GB ]|This will specify the languages to index.|
|siso_search.default.solr_spellcheck: true|This will enable or disable Solr spellcheck (aka "Did you mean?" functionality|
|siso_search.default.index_facet_fields:|Since Solr string fields are lowercased, this configuration is used to specify an additional solr _id field.</br>id fields are not lowercased, giving as result facets that can preserve their original case.</br>Please note that the field name should be specified without the _s suffix.|


## Custom indexing using plugins

Econtent indexer also supports a plugin architecture to add additional logic to the indexing process.

In the standard version there is a plugin to index price ranges. This is, given a product price, it will create a range index based on that price. I.E. if price = $150 the output could be 100-200

In order to work the plugins must implement this interface:

    EcontentIndexerPluginInterface

For additional information see the cookbook: [How to implement a custom indexer plugin for econtent](../../econtent_cookbook/econtent_search_cookbook/how_to_implement_a_custom_indexer_plugin_for_econtent.md)
