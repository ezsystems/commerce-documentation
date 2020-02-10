#  Catalog data providers - Products from different sources 

eZ Commerce provides a flexible way to handle products and catalogs. The shop uses central data providers to fetch data for the shop (concrete implementations of `CatalogDataProvider`).

![](attachments/23560468/23563691.png)

The blue colored section e.g. represents the products which  are located beneath the menu item "Products".

The products are dynamically injected into the content tree regardless of which source is used to provide the products.

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Catalog Data Provider
</th>
<th><div class="tablesorter-header-inner">
good for
</th>
<th>use case</th>
</tr>
</thead>
<tbody>
<tr>
<td><p>Catalog Data CMS eZ</p>
<p>(Ez5CatalogDataProvider)</p></td>
<td><p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" />  products can be edited in the CMS</p>
<p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" />  good for up to 20.000 products</p>
<p><img src="images/icons/emoticons/forbidden.png" class="emoticon emoticon-minus" alt="(minus)" /> import of products costs time</p></td>
<td><p>customer has no PIM system</p>
<p>and the amount of products is limited</p></td>
</tr>
<tr>
<td><p>Catalog Data eContent</p>
<p>(EcontentCatalogDataProvider)</p></td>
<td><p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /> allows more than 1 million products</p>
<p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /> fast imports</p>
<p><img src="images/icons/emoticons/forbidden.png" class="emoticon emoticon-minus" alt="(minus)" /> currently no edit interface in the backend of the CMS</p></td>
<td><p>The customer is using a PIM system</p>
<p>or the ERP provides all the product information</p></td>
</tr>
</tbody>
</table>

It is possible to implement own dataproviders. 

Currently the following catalog data providers are available:

  - Data provider for eZ Platform (`Ez5CatalogDataProvider`)
  - Data provider for econtent (`EcontentDataProvider`)

New data providers can be implemented using the documentation [Writing new catalog data providers](#)

## Attachments:

![](images/icons/bullet_blue.gif) [image2016-3-18 19:34:39.png](attachments/23560468/23563691.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-7-19 9:7:39.png](attachments/23560468/23563710.png) (image/png)  
