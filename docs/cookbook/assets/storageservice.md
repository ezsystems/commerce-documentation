# StorageService

`StorageService` provides utilities and a helper for interacting with files which are stored on the server.

Service ID: `silver_tools.storage_service`

The service fetches a list of files by provided $productId from `$storagePath`.

## getFilesByProductId

`getFilesByProductId` uses the [Symfony Finder component.](http://symfony.com/doc/3.4/components/finder.html)

The following example code:

``` php
use Silversolutions\Bundle\ToolsBundle\Services\StorageService;
 
$productId = '1002';
$storagePath = 'var/myStorage/';
 
$storageService = $this->get('silver_tools.storage_service');
$pdfFiles = $storageService->getFilesByProductId($productId, $storagePath, array('pdf'));
```

returns the following example structure:

``` 
// example output of var_dump($pdfFiles)
array (3) => (
    [0] => 
    string () "var/storage/manuals/1002.pdf"
    [1] => 
    string () "var/storage/manuals/1002_1.pdf"
    [2] => 
    string () "var/storage/manuals/1002_datasheet.pdf"
)
```
