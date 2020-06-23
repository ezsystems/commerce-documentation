# Indexing content model product data

If the content model is used as the data provider, the Solr Search Engine Bundle indexes all product data.
This means all indexed product documents are Content items.

In order to customize the searchable data for products, you can write plug-ins for the indexer of the Solr search bundle.

## ProductDocumentMapperPlugin

The `ProductDocumentMapperPlugin` plugin generates a special field for the price range based on product unit price.
It is a service which is injected into the main indexer execution based on the `ezpublish.search.solr.document_mapper_plugin` tag.

This service implements `EzSystems\EzPlatformSolrSearchEngine\DocumentMapperPluginInterface`.

### `canExtend()`

The `canExtend()` method checks and returns `true` if the passed Content Type can be extended.
For example, for products it checks for type `identifier == ses_products`

This is called in order to determine if this plugin implementation knows the given Content Type and is able to extend it.
If it returns true, `createExtensionFields()` is called, otherwise this plugin is ignored.

### `createExtensionFields()`

The `createExtensionFields()` method creates the SPI search elements that correspond to each product's extended fields.
This method receives a Content item, a Content Type and a language code.

It is called for every indexed Content item and language.
The generated Field instances get a prefix (e.g. `ext_`) in their names in order to avoid naming conflicts with existing content Fields
and to distinguish them from standard fields.
