#  Term - Dataprovider 

**Dataprovider** is the instance that provides the catalog data. eZ Commerce (Advanced version only) provides a flexible way to handle products and categories. Catalog data can be stored in different source in the shop (eZ, DB, solr) and the main goal of the data provider is to fetch the product data from the source and build the catalog elements.

Currently there are two concrete implementations for dataproviders:

<table>
<tbody>
<tr>
<td><p>Catalog Data CMS eZ</p>
<p>(Ez5CatalogDataProvider)</p></td>
<td><p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" />  products can be edited in the CMS</p>
<p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" />  good for up to 20.000 products</p>
<p><img src="images/icons/emoticons/forbidden.png" class="emoticon emoticon-minus" alt="(minus)" /> import of products costs time</p></td>
<td><p>customer has no PIM system</p>
<p>and the amount of roducts</p></td>
</tr>
<tr>
<td><p>Catalog Data eContent</p>
<p>(EcontentCatalogDataProvider)</p></td>
<td><p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /> allws more than 1 million products</p>
<p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /> fast imports</p>
<p><img src="images/icons/emoticons/forbidden.png" class="emoticon emoticon-minus" alt="(minus)" /> currently no edit interface in the backend of the CMS</p></td>
<td><p>The customer is using a PIM system</p>
<p>or the ERP provides all the product information</p></td>
</tr>
</tbody>
</table>
