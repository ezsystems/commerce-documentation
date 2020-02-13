#  Basket - Features 

The basket will offer a variety of features. Some of the features will be described in detail on separate pages.

## Overview

<table style="width:100%;">
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Store different kind of baskets</td>
<td><p>The basket can have several types. eZ Commerce provides</p>
<ul>
<li>basket</li>
<li>quickOrder</li>
<li>storedBasket</li>
<li>whishList</li>
<li>comparison</li>
</ul></td>
</tr>
<tr>
<td>A user can store a basket with a name</td>
<td>Is used e.g. for named basket of type "storedBasket"</td>
</tr>
<tr>
<td>Differentiate baskets between shops (Multishop)</td>
<td><p>Advanced version only</p>
<p>For each shop a shop_id can be configured. The shop_id will be stored in the basket</p></td>
</tr>
<tr>
<td>Stores the BuyerParty</td>
<td>The address of the customer (usually this address is the same as the InvoiceParty)</td>
</tr>
<tr>
<td>Stores the InvoiceParty</td>
<td><p>Address for the invoice from the checkout or customer profile</p></td>
</tr>
<tr>
<td>Stores the DeliveryParty</td>
<td>The delivery address given by the customer</td>
</tr>
<tr>
<td>Basket header - add fields</td>
<td><p>Additional fields can be placed in the basket header (dataMap Field)</p>
<p>This can be simple fields or complex data as well</p></td>
</tr>
<tr>
<td>Additional costs can be stored in the basket to implement all kind of additional costs or discounts</td>
<td><p>Additional costs or discounts such as</p>
<ul>
<li>shipping costs</li>
<li>costs for packaging</li>
<li>costs for payment</li>
<li>discount for vouchers</li>
<li>general discounts for total order</li>
</ul></td>
</tr>
<tr>
<td>A general remark for the basket</td>
<td>provided by the customer in the checkout</td>
</tr>
<tr>
<td>1..n basket lines</td>
<td><p>contains infos such as</p>
<ul>
<li>sku</li>
<li>variant code</li>
<li>quantity</li>
<li>prices</li>
<li>a remark or additional data per basketline. See <a href="Additional-data-in-the-basket-line_23560228.html">Additional data in the basket line</a></li>
</ul></td>
</tr>
<tr>
<td>Handling of discontinued products</td>
<td>See <a href="Handling-of-discontinued-products_23561074.html">Handling of discontinued products</a></td>
</tr>
<tr>
<td>performance improvment: The basketline stores a serialized version of the product in the basket line</td>
<td>A <a href="http://confluence.extranet.silversolutions.de:8090/display/EZC14/CatalogElementSerializeService" class="external-link">configuration</a> controls which fields from the product shall be stored</td>
</tr>
<tr>
<td>Calculation of prices if required</td>
<td><p>A <a href="Price-Engine_23560375.html">Price Engine</a> will be involved. It will calculate the prices and provide uptodate information about stock.</p>
<p>The basket contains a flag which controls if a recalculation has to be done (timeout).</p></td>
</tr>
<tr>
<td>Merging of baskets</td>
<td><p>After a login eZ Commerce merges the products from the existing basket (filled as a anonymous user) and a basket which is already stored for the given user id of the user.</p>
<p>If a sku is already present in a basket a new line will be created in the basket of the user.</p></td>
</tr>
<tr>
<td>Basket can be shared if a user logs in in different browsers (default) or bound to session only</td>
<td><p>In case a basket shall be not shared in case the same user logs in twice a setting can be adapted (default: false)</p>
<p>ses_basket.default.basketBySessionOnly: true</p></td>
</tr>
</tbody>
</table>
