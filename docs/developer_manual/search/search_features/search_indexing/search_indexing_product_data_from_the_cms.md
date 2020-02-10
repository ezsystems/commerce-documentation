#  Search - Indexing product data from the CMS 

If eZ Platform is used as a PIM (Product Information Management) system, the [Solr Search Engine Bundle](https://doc.ez.no/display/EZP/Solr+Search+Engine+Bundle) is used to index all product data. That means all indexed product documents are eZ content objects.

In order to customize the searchable data for products, it is possible to write plug-ins for the indexer of eZ's Solr search bundle. The API for these plug-ins is a work in progress and subject for changes.

## ProductDocumentMapperPlugin

This plugin generates special field for  the price range based on product unit price.

ProductDocumentMapperPlugin is a service which is inject in main indexer execution based on the tag: “ezpublish.search.solr.document\_mapper\_plugin"

This service implements EzSystems\\EzPlatformSolrSearchEngine\\DocumentMapperPluginInterface which is part of the Ez Search Bundle. As already mentioned, this interface is a subject to change.

The method canExtend must check and will return true if the passed content type can be extended. I.E. for products it will check for a type identifier  == ‘ses\_products’

<table>
<thead>
<tr class="header">
<th><pre><code>public function canExtend(Type $type) : bool</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><p>This is called in order to determine if this plugin implementation knows the given content type and is able to extend it.</p>
<p>If it returns true, createExtensionFields() is called respectively, otherwise this plugin is ignored.</p></td>
</tr>
</tbody>
</table>

The method createExtensionFields will create the SPI search elements that corresponds to each product extended fields. This method will receive a Content, a Type and a language code.

<table>
<thead>
<tr class="header">
<th><pre><code>public function createExtensionFields(Content $content, Type $type, string $languageCode) : eZ\Publish\SPI\Search\Field</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><p>This is called for every indexed content object and language.</p>
<p>The generated Field instances SHOULD get a prefix (e.g. ext_) in their names in order to avoid naming conflicts with existing content fields and to distinguish them from standard fields.</p></td>
</tr>
</tbody>
</table>
