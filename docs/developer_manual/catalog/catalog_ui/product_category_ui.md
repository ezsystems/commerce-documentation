#  Product category - UI 

eZ Commerce allows to show a product category page using different layouts:

``` 
#possible values: product_list, categories, both
siso_core.default.category_view: product_list
```

The categories are rendered using a template Catalog/Subrequests/listChildren.html.twig

Offered options: 

  - **product\_list: display product list directly**
  - **both: display subcategories and product list - here we may display the facets in the left sidebar instead of the navigation.**

  - **categories:  display only subcategories and only on the last category (if there are no subcategories) display the product list.**

## Data provided for categories

Please see  [`CatalogElement`](23560458.html) and `CatalogNode` for a detailed documentation.

**Product category (CatalogElement)**

**Predefined properties for `CatalogElement`**

Each `CatalogElement` has predefined properties. These methods are validated automated on constructor by the `validateProperties()` method.

<table>
<tbody>
<tr>
<td><strong>Identifier</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
</tr>
<tr>
<td><code>name</code></td>
<td><code>string</code></td>
<td>The name of the catalog</td>
</tr>
<tr>
<td><code>text</code></td>
<td><code>string</code></td>
<td>A short introduction text for the catalog</td>
</tr>
<tr>
<td><code>image</code></td>
<td><code>ImageField (FieldInterface)</code></td>
<td>An image for the catalog</td>
</tr>
<tr>
<td><code>path</code></td>
<td><code>array</code></td>
<td>The path of the catalog (array of identifier)</td>
</tr>
<tr>
<td><code>url</code></td>
<td><code>string</code></td>
<td>The internal URL of the catalog. This url should not be used for generating a link! Please use <code>seoUrl</code> instead</td>
</tr>
<tr>
<td><code>seoUrl</code></td>
<td><code>string</code></td>
<td>The human readable URL of the category</td>
</tr>
<tr>
<td><code>permanentUrl</code></td>
<td><code>string</code></td>
<td><p>The internal permanent URL</p></td>
</tr>
<tr>
<td><code>parentElementIdentifier</code></td>
<td><code>string</code></td>
<td>The unqique identifier of the parent catalog</td>
</tr>
<tr>
<td><code>identifier</code></td>
<td><code>string</code></td>
<td>The unqique identifier</td>
</tr>
<tr>
<td><code>dataMap</code></td>
<td><pre><code>FieldInterface[]</code></pre></td>
<td>A list of Field objects</td>
</tr>
<tr>
<td><pre><code>cacheIdentifier</code></pre></td>
<td><pre><code>int|string</code></pre></td>
<td>cache identifier of element to use as key in cache storage</td>
</tr>
</tbody>
</table>

There are 4 public methods to set properties: 

  - setImage, 
  - setName, 
  - setText, 
  - setCacheIdentifier
