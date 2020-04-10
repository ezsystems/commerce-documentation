# Additional indexing of eZ PDF content and other file content

## File Content Indexing

For the purpose of indexing the content of files stored in eZ it is been use an additional component called Apache Tika.

Apache Tika has two available modes: Server Mode and App Mode. We are using Apache Tika in App mode.

#### Which Files Are Indexed?

Currently there is a configuration to specify by mime type which files are indexed.

``` xml
# Specify the mimetype of file content that should be indexed.
siso_search.default.index_content:
    - application/pdf
```

If you need additional file types to be indexed you only have to add them to this configuration.

Example:

``` yaml
# Specify the mimetype of file content that should be indexed.
siso_search.default.index_content:
    - application/pdf
    - application/vnd.ms-excel
    - application/msword
```

Here is a list of Apache Tika supported formats:

http://tika.apache.org/1.13/formats.html

#### How to extend file content indexer

In order to extend the indexer for files you have to extend this service:

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/DatatypesBundle/FieldType/BinaryFile/SearchField.php`

The main method that returns the data to be indexed is:

``` 
public function getIndexData(Field $field, FieldDefinition $fieldDefinition)
```

`getIndexData()` returns an array of `\eZ\Publish\SPI\Search\Field`

This Field type is created with the following parameters:

name: which is a name that will be used to build Solr field name.

data: the data to be indexed.

Solr field type: you can use any Solr field type. In PDF example we are indexing the data both as FullTextField and as TextField.

``` php
$parentData = parent::getIndexData($field, $fieldDefinition);

// This will return the data that is coming from Apache Tika
$fileContent = $this->getFileContent($field);
 
// This will create a Solr FullTextField
$parentData[] = new Search\Field(
    'file_content',
    $fileContent,
    new Search\FieldType\FullTextField()
);
 
// This will create a Solr TextField
$parentData[] = new Search\Field(
    'file_content',
    $fileContent,
    new Search\FieldType\TextField()
);

return $parentData;
```

To know more about Solr field types you can check this Solr documentation.

https://cwiki.apache.org/confluence/display/solr/Field+Type+Definitions+and+Properties

String fields and Text fields should be visible as a search result in Solr web administration.

Full Text Fields are only visible in schema browser.

In this example Solr text field name for `file_content` is:

```
"file_file_file_content_t"
```

#### How information is been taken out of the file

The core of this indexer is Apache Tika, a component that can fetch a file and return its contents in plain text, or in HTML.

Apache Tika is injected as a service in the constructor of this class:

``` php
public function __construct($apacheTikaService)
{
    $this->apacheTikaService = $apacheTikaService;
}
```

The main method we use is getText, as you can see in this example:

``` php
$fileContent = $this->apacheTikaService->getText($fileName);
```

But there are more additional methods to get HTML or even metadata of the file.

``` 
getMetadata($fileName)
getHTML($fileName)
```

### PDF should be visible on Ez content search results

This means that if you search for any content of a PDF, the PDF eZ file element should appear as a search result on Ez content search result page.

### Things that can con wrong

Be sure Apache Tika Bundle is installed. This includes a bundle that should be enabled in Ez Kernel.

``` php
public function registerBundles()
{
    $bundles = array(
        ...
        ...
        new Joli\ApacheTikaBundle\ApacheTikaBundle(),
    );
}
```

This is the web site of the bundle: https://packagist.org/packages/jolicode/apache-tika-bundle

Make sure Apache Tika file is accessible and configured:

By default we have the file in bin directory

`config.yml`

`apache_tika_path: bin/tika-app-1.13.jar

`parameters.yml`

```
apache_tika: 
    path: '%kernel.root_dir%/../%apache_tika_path%'
```

This jar file can be downloaded from here:

http://www.apache.org/dyn/closer.cgi/tika/tika-app-1.13.jar

!!! caution

    Apache Tika log file is hardcoded. So make sure the file is writable both by command line user who executes the indexer and the apache user, because the indexer will be triggered after a file modification in the backend.

    If this file is not writable the indexer will not index file contents.

Apache Tika log file is located here: `/tmp/tika-error.log`
