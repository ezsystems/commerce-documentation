# Basket data model

The basket consists of two elements:

- a basket header containing the header information such as id's, info about the invoice party and delivery
- 1..n basket lines containing information about the products 

# Basket

!!! note

    Also check [Doctrine Datatypes](../../../cookbook/doctrine_mapping_datatypes.md).

The following attributes are provided by the basket header.

|Attribute|Meaning|Type (PHP)|Mandatory|
|--- |--- |--- |--- |
|basketId|Primary key. ID must be an integer since it will be passed to the ERP as well as in order to ensure that one order is placed just once. Some ERP systems accept just integer ID because of this purpose.|int|Yes|
|originId|Relation to the `originBasket`, primary key of the `originBasket` - if the basket is a copy of another basket with state 'new'|int||
|erpOrderId|ID assigned by the ERP to the order document|string||
|guid|ID which identifies the order uniquely across all Systems (e.g. ERP, Shop and Payment System)|string||
|state|`new`, `offered`, `payed`, `confirmed`|string|Yes|
|type|`basket`, `storedBasket`, `wishlist`|string|Yes|
|erpFailCounter|Used in the `processFailedOrder()` method of the `BasketService` in order to count the number of tries (of submitting the order to the ERP)|int||
|sessionId|`sessionId` of the anonymous user|string||
|userId|`userId` if user is logged in|int||
|basketName|The basket name given by user for function `storedBasket`. In this case the attribute `type` is `storedBasket` (see [Wishlist and stored baskets](../../wishlist_and_stored_baskets/wishlist_and_stored_baskets.md))|string||
|invoiceParty|Invoice address. Note: in order process the address may have to be updated if the customer changes the invoice address meanwhile (see Customers)|Party||
|deliveryParty|Delivery address chosen for the order (see Customers)|Party||
|buyerParty|Customer address.  Note: in order process the address may have to be updated if the customer changes the invoice address meanwhile (see Customers)|Party||
|remark|An additional customer remark|string||
|dateCreated|Datetime of the basket creation|Datetime|Yes|
|dateLastModified|Datetime of the basket last modification|Datetime|Yes|
|shop|Code of the shop where the order has been placed. The code is defined in the silver.e-shop.yml|string||
|requirePriceUpdate|If true, price engine must be initiated|boolean|Yes|
|totals|Stores `BasketTotals` object, used for all lines|array of BasketTotals||
|totalsSum|Stores an array of basketTotals objects, used by order splitting|BasketTotals||
|totalsSumNet|Basket total net sum as simple float value|float||
|totalsSumGross|Basket total gross sum as simple float value|float||
|currency|Basket currency|string||
|additionalLines|Array of `AdditionalLine` objects which can contain more information about products (eg. additional shipping costs, vouchers, another product for free, promotions)|array of AdditionalLine||
|lines|Array of BasketLine objects|array of BasketLine||
|messages|Contains the error, notice and success messages, these are not stored in the database|array||
|dateLastPriceCalculation|Contains date with last calculation for prices made by basket service when storing basket|DateTime||
|shippingMethod|The selected shipping method|string||
|paymentMethod|The selected payment method|string||
|paymentTransactionId|Contains transaction ID given by the payment provider|string||
|confirmationEmail|Email address where the order confirmation email will be sent|string||
|allProductsAvailable|True if all products in basket are available. Please not that this value can be checked only if a service such as the ERP provides Stockinfo. The logic is:</br>initially the value is set to true</br>all products are checked if if a stock info is available it is checked: if stock is not available allProductsAvailable will be set to false</br>If no stockinfo is provided at all the (e.g. there was no ERP request) value might be wrong (true)|boolean||
|dataMap|Stores additional information (e.g. project specific attributes)</br>Defined and optional values in dataMap:</br>orderType - will be displayed in the order list in the backend|array||


# BasketLine

|Attribute|Meaning|Type (PHP)|Mandatory|
|--- |--- |--- |--- |
|basketLineId|Primary key|int|Yes|
|lineNumber|Number from 1..n. The number is set when the user adds an article to the basket|int|Yes|
|sku|Unique article number|string|Yes|
|variantCode|If variant is used|string||
|productType|E.g. product, event, subscription, ebook, download.</br>This field is used e.g. in order to</br>choose a template for displaying the product</br>info for price engine: which request  shall be used to the ERP</br>Example: `OrderableProductNode`|string|Yes|
|quantity|Quantity of the product|float|Yes|
|unit|piece, kg, package|string||
|price|Price for one item of the line|float||
|priceNet|Net price for one item of the line|float||
|priceGross|Gross price for one item of the line|float||
|linePriceAmountNet|Net price of all items of the line|float||
|linePriceAmountGross|Gross price of all items of the line|float||
|vat|VAT in %|float||
|isIncVat|Is the price inc. VAT|boolean||
|currency|Line currency|string|Yes|
|catalogElement|ProductNode - must be orderable</br>When a new line is created, the complete (orderable) CatalogElement can be stored in BasketLine.</br>The process of the serializing of catalog element is done automatically by doctrine listener.|CatalogElement||
|groupCalc|Not used yet|string||
|groupOrder|Not used yet|string||
|remark|An additional remark of the user|string||
|basket|Relation to Basket object|Basket||
|remoteDataMap|Additional information for the given basket line. This field may contain extra data provided by the user for this basket line (e.g. a special field such as a length or a reference to an internal order number)</br>The price engine will set the following attributes in the remoteDataMap when provided by the price service (e.g. by an ERP system):</br>stockNumeric - number of items in stock</br>onStock - boolean|array||
|assignedLines|Array of AdditionalLine objects which can contain more information about products (eg. additional shipping costs, vouchers, another product for free, promotions)|array of AdditionalLine||
|variantCharacteristics|Contains all characteristics for given variantCode|array||

### BasketTotals

!!! note

    BasketTotals is not an Entity, therefore it has no table in the database. BasketTotals is stored in Basket as:

    1. serialize object - totalsSum
    1. serialize array of objects - totals

| Attribute  | Type (PHP) |
| ---------- | ---------- |
| totalNet   | float      |
| totalGross | float      |
| vatList    | array      |
| currency   | string     |

### Important Basket Repository methods

|Method|Usage|Parameters|
|--- |--- |--- |
|getBasketBySessionId|Get basket by SessionId|$sessionId, $state, $type, $name = null, $splittingcode = null|
|getBasketByUserId|Get basket by userId|$userId, $state, $type, $name = null, $splittingcode = null|
|removeAllAsignedBaskets|Removes all baskets from database that are based on the origin basket|$originBasketId|
|removeBasketsOlderThan|Removes all old anonymous baskets from the storage|$validationDatetime|

### Usage of BasketRepository Methods

#### Fetching/Removing Entities in BasketService

You can fetch/remove the entities from the database with the help of Doctrine EntityManager and the Repository functions.

``` php
$basket = $this->entityManager->getRepository('SilversolutionsEshopBundle:Basket')->getBasketByUserId($userId, $type, $state, $name, $splittingCode);
$entityManager->getRepository('SilversolutionsEshopBundle:Basket')->removeAllAsignedBaskets($basket->getBasketId());
```
