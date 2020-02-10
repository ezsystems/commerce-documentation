#  Catalog and Search 

## The catalog

The catalog is build using categories and products which are setup as eZ Content objects in the backend. The categories and products can be translated. 

**eContent for eZ Commerce Advanced**

In the Advanced version all categories and products are setup in eContent. eContent is a storage provider for eZ Commerce which can store product data in a very efficient way. It allows  to store data (mostly for products and product groups) in database tables with a simple structure.

  - Fast imports (e.g. from ERP or PIM systems)
  - Supports more than one million products
  - search ready
  - Fast access
  - avoids to store products in the CMS content model
  - eContent offers a staging feature and enhanced catalog segmentation features and allows imports during production and switching catalogs. 

Additionally a PIM system can be used to manage all product data. 

eZ Commerce is prepared to sell physical products. It can be enhanced by partners to sell other products such as digital products.

<table>
<thead>
<tr class="header">
<th>Feature</th>
<th>Details</th>
</tr>
</thead>
<tbody>
<tr>
<td>Categories/Product groups</td>
<td><div class="content-wrapper">
<p><img src="attachments/23561039/23571134.png" class="confluence-embedded-image" height="400" /></p>
<p>The 'category' represents a product group. eZ Commerce allows to show a product category page using different layouts. The layout can be configured in the backend.</p>
<p>3 different layouts are provided in the standard (display sub- categories, product list or both) on the entry page of a category) It can also be configured if best sellers of the group should be displayed. This is done in the configuration settings in the backend of the shop.</p>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Display product list directly </p></th>
<th><p>Display only subcategories</p>
(These can have several levels of subcategories. On the last level the product list is shown.)</th>
<th>Display subcategories and product list</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<p><img src="attachments/23561039/23571130.png" class="confluence-embedded-image" height="400" /></p>
</td>
<td><div class="content-wrapper">
<p><img src="attachments/23561039/23571129.png" class="confluence-embedded-image" width="434" /></p>
</td>
<td><div class="content-wrapper">
<p><img src="attachments/23561039/23571120.png" class="confluence-embedded-image" width="434" /></p>
</td>
</tr>
</tbody>
</table>

</td>
</tr>
<tr>
<td>Product list view</td>
<td><p>The product list offers two different views (grid and list). From the list view the user can directly add a product (if it is not a variant product) to basket, wishlist or comparison or go to the product detail page.<br />
 </p>
<p>The catalog shows filters depending on the displayed products and their attributes.<br />
</p></td>
</tr>
<tr>
<td>Product type</td>
<td><div class="content-wrapper">
<p>The product type (not to be confused with variants) represents a collection of very similar products, that differ only for some characteristics. It is used to show a list of products in a tabular way, every product can be added to basket directly from this overview page. </p>
<p><img src="attachments/23561039/23570901.png" class="confluence-embedded-image" height="400" /></p>
</td>
</tr>
<tr>
<td>Product detail</td>
<td><div class="content-wrapper">
<p><img src="attachments/23561039/23571121.png" class="confluence-embedded-image" height="250" /></p>
<p><img src="attachments/23561039/23563172.png" class="confluence-embedded-image" width="434" /></p>
<p>A product can have:</p>
<ul>
<li>one or more images (in the standard config up to 3),</li>
<li>a SKU</li>
<li>text fields (name, description, intro)</li>
<li>specifications offering different groups of data as marketing data, technical data, others depending on the data that have been created in the backend<br />
<br />
</li>
</ul>
<p>From the product detail the user can directly add a product to basket, wishlist, comparison or stored basket.</p>
</td>
</tr>
<tr>
<td>Variant product</td>
<td><div class="content-wrapper">
<p>eZ Commerce supports variants with 1 or 2 levels.<br />
The user can choose a first attibute, the shop will then narrow down the options for the second level. <strong>  </strong></p>
<p><br />
</p>
<p><img src="attachments/23561039/23563077.png" class="confluence-embedded-image" height="400" /></p>
<p>The user can change the variant selection in the basket.</p>
<p><img src="attachments/23561039/23563169.png" class="confluence-embedded-image" width="434" /></p>
</td>
</tr>
<tr>
<td>Product comparison</td>
<td><div class="content-wrapper">
<p>A customer can compare products in a comparison list. The comparison list automatically groups products per product category (so e.g. mixer cannot be compared to microwave).<br />
The customer can change the order of the sorting per drag&amp;drop and he can decide to display only differences of the products.</p>
<p><img src="attachments/23561039/23571126.png" class="confluence-embedded-image" width="434" /></p>
</td>
</tr>
<tr>
<td>Whishlist</td>
<td><div class="content-wrapper">
<p>After login customers can add products to a personal whishlist. Products that are not in the active catalog any more are automatically marked as "not available".</p>
<p><img src="attachments/23561039/23571123.png" class="confluence-embedded-image" height="250" /></p>
</td>
</tr>
<tr>
<td>Stored baskets</td>
<td><div class="content-wrapper">
<p>Customers with a login can store named baskets and can reuse them later on for recurring orders.</p>
<p><img src="attachments/23561039/23571122.png" class="confluence-embedded-image" height="250" /></p>
</td>
</tr>
</tbody>
</table>

## Search and filtering

All data such as products and content is indexed in a powerful search engine (Solr). 

<table>
<thead>
<tr class="header">
<th>Feature</th>
<th>Details</th>
</tr>
</thead>
<tbody>
<tr>
<td>Search</td>
<td><div class="content-wrapper">
<p>The integrated search engine offers one global search for products and content in the system:</p>
<ul>
<li>products </li>
<li>content such as blog entries, news, articles</li>
<li>assets such as downloads</li>
</ul>
<p>It respects the rights and roles of the customer.</p>
<p><img src="attachments/23561039/23571124.png" class="confluence-embedded-image" height="400" /><img src="attachments/23561039/23571127.png" class="confluence-embedded-image" height="400" /></p>
<p><br />
</p>
<p><br />
</p>
</td>
</tr>
<tr>
<td>Filters and facets</td>
<td><div class="content-wrapper">
<p>Almost all product fields can be used as a facet if they are indexed. This can be controlled by a configuration file which allows to setup different facets for the product catalog and search.</p>
<p>Facets can be grouped. For more complex facets new fields can be implemented using the IndexPlugin features of Solr platform search.</p>
<p><strong>Note</strong>: There are restrictions for variants, it is not possible to mix the variant attributes as facet if the same attribute is set for main products.</p>
<p><img src="attachments/23561039/23568693.png" class="confluence-embedded-image" height="400" /></p>
</td>
</tr>
<tr>
<td>Autosuggest</td>
<td><div class="content-wrapper">
<p>The autosuggest feature offers suggestion as you type and allows to add an orderable product to the basket directly from the search box. It suggests products, categories, downloads and content. The autosuggest matches products where the name starts with the searchterm.</p>
<p><img src="attachments/23561039/23571128.png" class="confluence-embedded-image" height="400" /></p>
</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-3-26\_13-9-45.png](attachments/23561039/23570992.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-26\_13-12-8.png](attachments/23561039/23570991.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-26\_13-12-54.png](attachments/23561039/23570995.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-26\_14-43-16.png](attachments/23561039/23570994.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-26\_14-44-37.png](attachments/23561039/23570993.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-27\_10-52-33.png](attachments/23561039/23570967.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-27\_10-59-29.png](attachments/23561039/23570968.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-27\_19-59-55.png](attachments/23561039/23570965.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-27\_20-0-45.png](attachments/23561039/23570966.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-27\_20-1-44.png](attachments/23561039/23570960.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-27\_20-5-2.png](attachments/23561039/23570963.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-27\_20-7-30.png](attachments/23561039/23570962.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-3-27\_20-30-34.png](attachments/23561039/23570964.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_18-26-27.png](attachments/23561039/23570901.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-01-31 um 10.12.17.png](attachments/23561039/23570881.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-01-31 um 10.11.10.png](attachments/23561039/23570882.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-01-31 um 10.10.48.png](attachments/23561039/23570900.png) (image/png)  
![](images/icons/bullet_blue.gif) [product\_catalog\_2.png](attachments/23561039/23568861.png) (image/png)  
![](images/icons/bullet_blue.gif) [prodcut list.png](attachments/23561039/23571130.png) (image/png)  
![](images/icons/bullet_blue.gif) [Subcategories.png](attachments/23561039/23571129.png) (image/png)  
![](images/icons/bullet_blue.gif) [both.png](attachments/23561039/23571120.png) (image/png)  
![](images/icons/bullet_blue.gif) [product.png](attachments/23561039/23571121.png) (image/png)  
![](images/icons/bullet_blue.gif) [product\_variant.png](attachments/23561039/23571125.png) (image/png)  
![](images/icons/bullet_blue.gif) [comparison list.png](attachments/23561039/23571126.png) (image/png)  
![](images/icons/bullet_blue.gif) [wishlist.png](attachments/23561039/23571123.png) (image/png)  
![](images/icons/bullet_blue.gif) [stored basket.png](attachments/23561039/23571122.png) (image/png)  
![](images/icons/bullet_blue.gif) [search\_1.png](attachments/23561039/23571124.png) (image/png)  
![](images/icons/bullet_blue.gif) [search\_2.png](attachments/23561039/23571127.png) (image/png)  
![](images/icons/bullet_blue.gif) [autosuggest.png](attachments/23561039/23568632.png) (image/png)  
![](images/icons/bullet_blue.gif) [ptoduct detail.png](attachments/23561039/23563172.png) (image/png)  
![](images/icons/bullet_blue.gif) [Change variant in basket.png](attachments/23561039/23563169.png) (image/png)  
![](images/icons/bullet_blue.gif) [product detail.png](attachments/23561039/23563077.png) (image/png)  
![](images/icons/bullet_blue.gif) [product\_catalog\_2.png](attachments/23561039/23568863.png) (image/png)  
![](images/icons/bullet_blue.gif) [product\_catalog\_2.png](attachments/23561039/23571134.png) (image/png)  
![](images/icons/bullet_blue.gif) [Filter\_Facetts.png](attachments/23561039/23568693.png) (image/png)  
![](images/icons/bullet_blue.gif) [autosuggest.png](attachments/23561039/23571128.png) (image/png)  
