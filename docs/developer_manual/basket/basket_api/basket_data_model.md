#  Basket data model 

The basket consists of two elements:

  - a basket header containing the header information such as id´s, info about the invoice party and delivery
  - 1..n basket lines containing infos about the products 

# Basket

Please check also [Doctrine Datatypes\!](Doctrine-Mapping-Datatypes_23560781.html)

The following attributes are provided by the basket header.

<table>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Meaning</th>
<th>Type (PHP)</th>
<th>Mandatory</th>
</tr>
</thead>
<tbody>
<tr>
<td>basketId</td>
<td>primary key. Id must be an integer since the Id will be passed to the ERP as well as in order to ensure that one order is placed just once. Some ERP systems accept just integers Id because of this purpose.</td>
<td>int</td>
<td>Yes</td>
</tr>
<tr>
<td>originId</td>
<td>relation to the originBasket, primary key of the originBasket - if the basket is a copy of another basket with state 'new'</td>
<td>int</td>
<td><br />
</td>
</tr>
<tr>
<td>erpOrderId</td>
<td><p>ID assigned by the ERP to the order document</p></td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>guid</td>
<td>ID, which identifies the order uniquely across all Systems (e.g. ERP, Shop and Payment System)</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>state</td>
<td>new, offered, payed, confirmed</td>
<td>string</td>
<td>Yes</td>
</tr>
<tr>
<td>type</td>
<td>basket, storedBasket, wishlist</td>
<td>string</td>
<td>Yes</td>
</tr>
<tr>
<td>erpFailCounter</td>
<td>used in the processFailedOrder() method of the BasketService in order to count the number of tries (of submitting the order to the ERP)</td>
<td>int</td>
<td><br />
</td>
</tr>
<tr>
<td>sessionId</td>
<td>sessionId of the anonymous user</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>userId</td>
<td>userId if user is logged in</td>
<td>int</td>
<td><br />
</td>
</tr>
<tr>
<td>basketName</td>
<td>the basket name given by user for function storedBasket. In this case the attribute 'type' is 'storedBasket' (see <a href="23560656.html">Wishlist &amp; Stored Baskets</a>)</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>invoiceParty</td>
<td>invoice address. Note: in order process the address may have to be updated if the customer changes the invoice address meanwhile (see <a href="Customers_23560704.html">Customers</a>)</td>
<td>Party</td>
<td><br />
</td>
</tr>
<tr>
<td>deliveryParty</td>
<td>delivery address chosen for the order (see <a href="Customers_23560704.html">Customers</a>)</td>
<td>Party</td>
<td><br />
</td>
</tr>
<tr>
<td>buyerParty</td>
<td>customer address.  Note: in order process the address may have to be updated if the customer changes the invoice address meanwhile (see <a href="Customers_23560704.html">Customers</a>)</td>
<td>Party</td>
<td><br />
</td>
</tr>
<tr>
<td>remark</td>
<td>an additional customer remark</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>dateCreated</td>
<td>datetime of the basket creation</td>
<td>Datetime</td>
<td>Yes</td>
</tr>
<tr>
<td>dateLastModified</td>
<td>datetime of the basket last modification</td>
<td>Datetime</td>
<td>Yes</td>
</tr>
<tr>
<td>shop</td>
<td>Code of the shop where the order has been placed. The code is defined in the silver.e-shop.yml (<a href="Term---shop_id_23560953.html">Term - shop_id</a>)</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>requirePriceUpdate</td>
<td>if true, price engine must be initiated</td>
<td>boolean</td>
<td>Yes</td>
</tr>
<tr>
<td>totals</td>
<td>stores BasketTotals object, used for all lines</td>
<td>array of BasketTotals</td>
<td><br />
</td>
</tr>
<tr>
<td>totalsSum</td>
<td>stores an array of basketTotals objects, used by order splitting</td>
<td>BasketTotals</td>
<td><br />
</td>
</tr>
<tr>
<td>totalsSumNet</td>
<td>Basket total net sum as simple float value</td>
<td>float</td>
<td><br />
</td>
</tr>
<tr>
<td>totalsSumGross</td>
<td>Basket total gross sum as simple float value</td>
<td>float</td>
<td><br />
</td>
</tr>
<tr>
<td>currency</td>
<td>Basket currency</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>additionalLines</td>
<td>Array of AdditionalLine objects which can contain more information about products (eg. additional shipping costs, vouchers, another product for free, promotions)</td>
<td>array of AdditionalLine</td>
<td><br />
</td>
</tr>
<tr>
<td>lines</td>
<td>Array of BasketLine objects</td>
<td>array of BasketLine</td>
<td><br />
</td>
</tr>
<tr>
<td><a href="Messages_23560391.html">messages</a></td>
<td>contains the error, notice and success messages, these are not stored in the database</td>
<td>array</td>
<td><br />
</td>
</tr>
<tr>
<td>dateLastPriceCalculation</td>
<td>contains date with last calculation for prices made by basket service when storing basket</td>
<td>DateTime</td>
<td><br />
</td>
</tr>
<tr>
<td>shippingMethod</td>
<td>choosen shipping method</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>paymentMethod</td>
<td>choosen payment method</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>paymentTransactionId</td>
<td>contains transaction Id given by the payment provider</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>confirmationEmail</td>
<td>email address where the order confirmation email will be sent</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>allProductsAvailable</td>
<td><div class="content-wrapper">
<p>true if all products in basket are available. Please not that this value can be checked only if a service such as the ERP provides Stockinfo. The logic is:</p>
<ul>
<li>initially the value is set to true</li>
<li>all products are checked if if a stock info is available it is checked: if stock is not available allProductsAvailable will be set to false</li>
</ul>

<p>If no stockinfo is provided at all the (e.g. there was no ERP request) value might be wrong (true)</p>
</td>
<td>boolean</td>
<td><br />
</td>
</tr>
<tr>
<td>dataMap</td>
<td><p> stores additional information (e.g. project specific attributes)</p>
<p>Defined and optional values in dataMap:</p>
<ul>
<li>orderType - will be displayed in the order list in the backend</li>
</ul></td>
<td>array</td>
<td><br />
</td>
</tr>
</tbody>
</table>
# BasketLine

<table>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Meaning</th>
<th>Type (PHP)</th>
<th>Mandatory</th>
</tr>
</thead>
<tbody>
<tr>
<td>basketLineId</td>
<td>primary key</td>
<td>int</td>
<td>Yes</td>
</tr>
<tr>
<td>lineNumber</td>
<td>number from 1..n. The number is set when the user add an article to the basket</td>
<td>int</td>
<td>Yes</td>
</tr>
<tr>
<td>sku</td>
<td>unique article number</td>
<td>string</td>
<td>Yes</td>
</tr>
<tr>
<td>variantCode</td>
<td>if variant is used</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>productType</td>
<td><p>e.g. product, event, subscription, ebook, download.</p>
<p>This field is used e.g. in order to</p>
<ul>
<li>choose a template for displaying the product</li>
<li>info for price engine: which request  shall be used to the ERP</li>
</ul>
<p>Example: OrderableProductNode</p></td>
<td>string</td>
<td>Yes</td>
</tr>
<tr>
<td>quantity</td>
<td>quantity of the product</td>
<td>float</td>
<td>Yes</td>
</tr>
<tr>
<td>unit</td>
<td>piece, kg, package</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>price</td>
<td>price for one item of the line</td>
<td>float</td>
<td><br />
</td>
</tr>
<tr>
<td>priceNet</td>
<td>net price for one item of the line</td>
<td>float</td>
<td><br />
</td>
</tr>
<tr>
<td>priceGross</td>
<td>gross price for one item of the line</td>
<td>float</td>
<td><br />
</td>
</tr>
<tr>
<td>linePriceAmountNet</td>
<td>net price of all items of the line</td>
<td>float</td>
<td><br />
</td>
</tr>
<tr>
<td>linePriceAmountGross</td>
<td>gross price of all items of the line</td>
<td>float</td>
<td><br />
</td>
</tr>
<tr>
<td>vat</td>
<td>vat in %</td>
<td>float</td>
<td><br />
</td>
</tr>
<tr>
<td>isIncVat</td>
<td>is the price inc. VAT</td>
<td>boolean</td>
<td><br />
</td>
</tr>
<tr>
<td>currency</td>
<td>line currency</td>
<td>string</td>
<td>Yes</td>
</tr>
<tr>
<td>catalogElement</td>
<td><p>ProductNode - must be orderable</p>
<p>When a new line is created, the complete (orderable) CatalogElement can be stored in BasketLine.</p>
<p>The process of the serializing of catalog element is done automatically by <a href="Storing-parts-of-the-product-in-the-basket-line_23560572.html">doctrine listener</a>.</p></td>
<td>CatalogElement</td>
<td><br />
</td>
</tr>
<tr>
<td>groupCalc</td>
<td>not used yet</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>groupOrder</td>
<td>not used yet</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>remark</td>
<td>an additional remark of the user</td>
<td>string</td>
<td><br />
</td>
</tr>
<tr>
<td>basket</td>
<td>relation to Basket object</td>
<td>Basket</td>
<td><br />
</td>
</tr>
<tr>
<td>remoteDataMap</td>
<td>additional information for the given basket line. This field may contain extra data provided
<p> by the user for this basket line (e.g. a special field such as a length or a reference to an internal order number)</p>
<p>The price engine will set the following attributes in the remoteDataMap when provided by the price service (e.g. by an ERP system):</p>
<ul>
<li>stockNumeric - number of items in stock</li>
<li>onStock - boolean</li>
</ul></td>
<td>array</td>
<td><br />
</td>
</tr>
<tr>
<td>assignedLines</td>
<td>Array of AdditionalLine objects which can contain more information about products (eg. additional shipping costs, vouchers, another product for free, promotions)</td>
<td>array of AdditionalLine</td>
<td><br />
</td>
</tr>
<tr>
<td><a href="http://confluence.ng.silverproducts.de/display/EX/Product+Variants#ProductVariants-Characteristics" class="external-link">variantCharacteristics</a></td>
<td>contains all characteristics for given variantCode</td>
<td>array</td>
<td><br />
</td>
</tr>
</tbody>
</table>

### BasketTotals

BasketTotals is not an Entity, therefore there is no own table in the database. BasketTotals is stored in Basket as

1.  serialize object - totalsSum
2.  serialize array of objects - totals

| Attribute  | Type (PHP) |
| ---------- | ---------- |
| totalNet   | float      |
| totalGross | float      |
| vatList    | array      |
| currency   | string     |

### Important Basket Repository methods

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Method
</th>
<th><div class="tablesorter-header-inner">
Usage
</th>
<th><div class="tablesorter-header-inner">
Parameters
</th>
</tr>
</thead>
<tbody>
<tr>
<td>getBasketBySessionId</td>
<td>get basket by SessionId</td>
<td>$sessionId, $state, $type, $name = null, $splittingcode = null</td>
</tr>
<tr>
<td>getBasketByUserId</td>
<td>get basket by userId</td>
<td>$userId, $state, $type, $name = null, $splittingcode = null</td>
</tr>
<tr>
<td>removeAllAsignedBaskets</td>
<td>removes all baskets from database that are based on the origin basket</td>
<td>$originBasketId</td>
</tr>
<tr>
<td>removeBasketsOlderThan</td>
<td>removes all old anonymous baskets from the storage</td>
<td>$validationDatetime</td>
</tr>
</tbody>
</table>

### Usage of BasketRepository Methods:

#### Fetching/Removing Entities in BasketService

You can fetch/remove the entities from the database with the help of Doctrine EntityManager and the Repository functions.

``` 
$basket = $this->entityManager->getRepository('SilversolutionsEshopBundle:Basket')->getBasketByUserId($userId, $type, $state, $name, $splittingCode);
$entityManager->getRepository('SilversolutionsEshopBundle:Basket')->removeAllAsignedBaskets($basket->getBasketId());
```
