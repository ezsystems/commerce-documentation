#  ProductType 

The **ProductType** extends [**CatalogElement**](23560458.html) and implements **ProductNodeContainerInterface**.

This class defines the methods needed to instantiate a product type.

### Properties for ProductType

<table>
<thead>
<tr class="header">
<th>Identifier</th>
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>childProducts</code></pre></td>
<td><pre><code>CatalogElement[]</code></pre></td>
<td>Array of Products that belong to this product type</td>
</tr>
<tr>
<td><pre><code>shortDescription</code></pre></td>
<td><pre><code>TextBlockField (FieldInterface)</code></pre></td>
<td>Short description</td>
</tr>
<tr>
<td><pre><code>longDescription</code></pre></td>
<td><pre><code>TextBlockField (FieldInterface)</code></pre></td>
<td>Description</td>
</tr>
<tr>
<td><pre><code>specifications</code></pre></td>
<td><pre><code>AbstractField[]</code></pre></td>
<td>A list of specifications of a product</td>
</tr>
<tr>
<td><pre><code>imageList</code></pre></td>
<td><pre><code>ImageField[] (FieldInterface[])</code></pre></td>
<td>A list of images</td>
</tr>
<tr>
<td><pre><code>displayInSearch</code></pre></td>
<td><pre><code>bool</code></pre></td>
<td>True if the productType should be displayed in the search result</td>
</tr>
<tr>
<td><pre><code>displayInProductList</code></pre></td>
<td><pre><code>bool</code></pre></td>
<td>True if the productType should be displayed in the product list restul</td>
</tr>
</tbody>
</table>

### Methods

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Parameters</th>
<th>Return value</th>
<th>Throws</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>getChildProducts</code></pre></td>
<td><br />
</td>
<td><pre><code>CatalogElement[]</code></pre></td>
<td><br />
</td>
<td>Returns all child products</td>
</tr>
<tr>
<td><pre><code>addChildProducts</code></pre></td>
<td><pre><code>ProductNode[]</code></pre></td>
<td><br />
</td>
<td><pre><code>InvalidArgumentException</code></pre></td>
<td>Adds the products passed as argument to the list of child products</td>
</tr>
<tr>
<td><pre><code>hasChildProducts</code></pre></td>
<td><br />
</td>
<td><pre><code>int</code></pre></td>
<td><br />
</td>
<td>Returns the number of children</td>
</tr>
<tr>
<td><pre><code>getChildProductBySku</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>CatalogElement</code></pre></td>
<td><br />
</td>
<td>Returns the child product that has the SKU passed as argument</td>
</tr>
<tr>
<td><pre><code>addChildProduct</code></pre></td>
<td><pre><code>ProductNode</code></pre></td>
<td><br />
</td>
<td><br />
</td>
<td>Add a single product to the list of child products</td>
</tr>
</tbody>
</table>
