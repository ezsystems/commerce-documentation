# StorageService

Utilities and helper for interacting with files, which are stored on the server.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>StorageService</th>
<th><br />
</th>
<th>Parameters</th>
<th>Return</th>
</tr>
</thead>
<tbody>
<tr>
<td>Id</td>
<td>silver_tools.storage_service</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>Meaning</td>
<td>fetches a list of files by a provided $productId from $storagePath</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>Function</td>
<td>GetFilesByProductId()</td>
<td><p>string $productId</p>
<p>string $storagePath</p>
<p>string $suffix = ''</p>
<p>int $subFolder = 0</p>
<p>string $separator = '_'</p></td>
<td><p>array of strings - path names of the existing files</p>
<p>or an empty array</p></td>
</tr>
</tbody>
</table>

## public function getFilesByProductId($productId, $storagePath, $suffix = '', $subFolder = 0, $separator = '\_')

This makes use of the Symfony-Component 'Finder'.  
See: <http://symfony.com/doc/current/components/finder.html>

### Usage

The example code:

``` 
use Silversolutions\Bundle\ToolsBundle\Services\StorageService;
 
$productId = '1002';
$storagePath = 'var/myStorage/';
 
$storageService = $this->get('silver_tools.storage_service');
$pdfFiles = $storageService->getFilesByProductId($productId, $storagePath, array('pdf'));
```

will return the following exemplary structure:

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

### Documentation

``` 
    /**
     * Fetches a list of files by a provided $productId from $storagePath.
     *
     * $productId is the complete identification of a single product. It could
     * contain a variant-code or similar codes (e.g. 'abc', '08/15', 'a02-04')
     *
     * $storagePath is the root path to where files are stored.
     *
     * If $suffix is not empty, only files includes in $suffix will be matched.
     * $suffix can be an array of strings or a single string.
     *
     * If $subFolder is not 0 (zero), the first n characters of $productId,
     * are used as a part of the paths.
     *
     * For example: getFilesByProductId('abcd', '/media/', array('jpg', 'pdf'), '3')
     * would match: '/media/a/b/c/abcd.jpg', '/media/a/b/c/abcd_1.jpg'
     * but NOT    : '/media/abcd.pdf', '/media/a/b/abcd_big.jpg'
     *
     * The $separator is used to identify multiple files for a single $productId.
     *
     * For example: getFilesByProductId('product-manuals', '/media/', 'pdf', 0, '-')
     * would match: '/media/product-manuals.pdf', '/media/product-manuals-v2.pdf'
     * but NOT    : '/media/product-reference.pdf', '/media/product-2.pdf'
     *
     * Will return an array filled with of paths (as strings) or and empty array.
     *
     * @param string $productId    The complete product id
     * @param string $storagePath  Path to the beginning of the storage
     * @param array|string $suffix Suffixes to filter (include)
     * @param int $subFolder       Number of character of $productId to use as path
     * @param string $separator    Separator for multiple files per $productId
     *
     * @throws \InvalidArgumentException When $productId is not a string or empty
     * @throws \InvalidArgumentException When $storagePath is not a string, empty or not a readable path
     * @throws \InvalidArgumentException When $suffix is not a string or array of strings
     * @throws \InvalidArgumentException When $subFolder is not an integer or is negative
     * @throws \InvalidArgumentException When $separator is not a string or is empty
     *
     * @return array Array of strings of path names of existing files or empty array
     */
    public function getFilesByProductId($productId, $storagePath, $suffix = '', $subFolder = 0, $separator = '_')
   
```
