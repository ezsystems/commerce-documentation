#  Manage products, prices and stock 

This section is only relevant if eZ is used as product data storage. If an ERP is integrated - Advanced version only  - this data will be coming fro the ERP and eventually a PIM.

<table>
<thead>
<tr class="header">
<th>Feature</th>
<th>Details</th>
</tr>
</thead>
<tbody>
<tr>
<td>Manage categories</td>
<td><div class="content-wrapper">
<p>The product catalog is fully integrated into the content tree of the CMS.</p>
<p><img src="attachments/23561048/23570918.png" class="confluence-embedded-image" height="400" /></p>
<p>Products and categories can be assigned to the product catalog.</p>
<p><img src="attachments/23561048/23563130.png" class="confluence-embedded-image" height="400" /></p>
</td>
</tr>
<tr>
<td>Flexible categories</td>
<td><div class="content-wrapper">
<p>A category contains a standard set of fields. A category is a CMS content type and can be extended with additional fields by the eZ Partner.</p>
<p><img src="attachments/23561048/23570917.png" class="confluence-embedded-image" height="400" /></p>
</td>
</tr>
<tr>
<td>Product - product texts</td>
<td><div class="content-wrapper">
<p>eZ Commerce offers a set of product fields. Since a product is build using the flexible eZ content type system it can be extended with new fields easiliy.</p>
<p><img src="attachments/23561048/23570931.png" class="confluence-embedded-image" height="400" /></p>
<p><br />
</p>
</td>
</tr>
<tr>
<td>Product - flexible attribute system</td>
<td><div class="content-wrapper">
<p>The attribute system allows to manage product attributes in a fast and flexible way.</p>
<p>Attributes can be setup using a template system (shared attributes) or extended with product specific attributes.</p>
<p>The attributes can be grouped.</p>
<p><img src="attachments/23561048/23570930.png" class="confluence-embedded-image" height="400" /></p>
<p>Product attributes are indexed in the search engine as well and can be used for facet search. </p>
<p><br />
</p>
</td>
</tr>
<tr>
<td><p>Product - Variants</p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<p>eZ Commerce offers one or two level variants. A variant can be added directly on the product view.</p>
<p>There are 3 preconfigured variant types:  Color, Size, Color &amp; Size</p>
<p>The list of variant types can be extended in the configuration by the eZ Partner.  </p>
<p><br />
</p>
<p><img src="attachments/23561048/23570933.png" class="confluence-embedded-image" height="400" /></p>
</td>
</tr>
<tr>
<td>Preview mode for products</td>
<td><div class="content-wrapper">
<p>While editing a product the preview function allows to display exactly how the product will look on desktops or mobile devices including product variants.</p>
<p>This simplifies the work of a shop owner/editor since he can see how a complex product would look before it is published in the shop.</p>
<p><img src="attachments/23561048/23567756.png" class="confluence-embedded-image" height="400" /></p>
</td>
</tr>
<tr>
<td>Product assets - images, videos and pdfs</td>
<td><div class="content-wrapper">
<p>eZ Commerce supports assets per product (not per variant). In the standard</p>
<ul>
<li>one main image</li>
<li>up to 3 additional images</li>
<li>one pdf</li>
<li>one link to a video (e.g. YouTube)</li>
</ul>
<p><br />
</p>
<p><img src="attachments/23561048/23570935.png" class="confluence-embedded-image" height="400" /></p>
<p>eZ Commerce is using the image system of the eZ Platform CMS including the features to scale images in different resolutions. </p>
</td>
</tr>
<tr>
<td>Product - stock</td>
<td><div class="content-wrapper">
<p>The stock can be managed by sku and sku/variant. This can be done manually or per upload.</p>
<p>In addition a stock text can be added (e.g. not on stock but will be back on 1st April). After an order is placed the stock will be reduced by the number of products bought by the customer.</p>
<p>Ther are two places where stock and prices can be managed:</p>
<p>1) In the product on the tab "eCommerce":</p>
<p><img src="attachments/23561048/23563074.png" class="confluence-embedded-image" width="750" /></p>
<p><br />
</p>
<p>2) In the section "eCommerce" under "Price and Stock Management":</p>
<p><img src="attachments/23561048/23567742.png" class="confluence-embedded-image" /></p>
</td>
</tr>
<tr>
<td>Product prices</td>
<td><div class="content-wrapper">
<p>The price management allows to setup prices manually. A price can be setup per sku and sku/variant. Each price can contain an offer price and a base price. If an offer price is set it will be displayed in the shop as</p>
<p><del>Old price: 10.00 €</del> New price: 9.80 €</p>
<p>In addition prices can vary per customer group. There are 3 customer goups set up per default. </p>
<p><img src="attachments/23561048/23563164.png" class="confluence-embedded-image" width="700" /></p>
<p>Currencies in eZ Commerce:</p>
<p> <a href="eCommerce-Administration_23561049.html">The currency is configured per country in the configuration settings.</a></p>
<ul>
<li>if a product has a price for a product and a currency this price will be displayed in the shop</li>
<li>if no price for a currency is set in a shop eZ Commerce offers 2 options, these can be set in the configuration per shop.<br />

<ul>
<li>Calculate the price for the requested currency using the base price defined in the product (using the base currency setup for the installation) and an exchange rate defined in the configuration</li>
<li>Display an error in the frontend that no price is available<br />
<br />
</li>
</ul></li>
</ul>
</td>
</tr>
<tr>
<td>Import/export stock and price information</td>
<td><div class="content-wrapper">
<p>This feature allows to update stock and prices using a csv file. Prices and stock can be downloaded and uploaded.</p>
<p><img src="attachments/23561048/23567753.png" class="confluence-embedded-image" height="400" /></p>
</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-4-4\_19-7-36.png](attachments/23561048/23570915.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-8-38.png](attachments/23561048/23570916.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-10-52.png](attachments/23561048/23570918.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-14-10.png](attachments/23561048/23570917.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-17-33.png](attachments/23561048/23570931.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-19-50.png](attachments/23561048/23570930.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-22-0.png](attachments/23561048/23570933.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-26-14.png](attachments/23561048/23570935.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-29-39.png](attachments/23561048/23570939.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-4\_19-31-6.png](attachments/23561048/23570937.png) (image/png)  
![](images/icons/bullet_blue.gif) [FireShot Screen Capture \#019 - 'eZ Platform' - ezcommerce-demo\_ezplatform\_com\_admin\_content\_edit\_draft\_270\_3\_eng-GB.png](attachments/23561048/23571163.png) (image/png)  
![](images/icons/bullet_blue.gif) [Stock management.png](attachments/23561048/23571164.png) (image/png)  
![](images/icons/bullet_blue.gif) [Product price\_Currency.png](attachments/23561048/23571160.png) (image/png)  
![](images/icons/bullet_blue.gif) [Price management.png](attachments/23561048/23563164.png) (image/png)  
![](images/icons/bullet_blue.gif) [catalog.png](attachments/23561048/23563130.png) (image/png)  
![](images/icons/bullet_blue.gif) [stock and price per product.png](attachments/23561048/23563074.png) (image/png)  
![](images/icons/bullet_blue.gif) [stock managment.png](attachments/23561048/23563075.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-11\_19-45-3.png](attachments/23561048/23571068.png) (image/png)  
![](images/icons/bullet_blue.gif) [Product price management.png](attachments/23561048/23567742.png) (image/png)  
![](images/icons/bullet_blue.gif) [Import\_export\_prices.png](attachments/23561048/23567753.png) (image/png)  
![](images/icons/bullet_blue.gif) [Product\_preview\_mode.png](attachments/23561048/23567756.png) (image/png)  
