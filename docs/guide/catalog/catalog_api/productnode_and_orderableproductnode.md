# ProductNode and OrderableProductNode

The `ProductNode` is the abstract base class for product elements for all concrete product nodes. It inherits from `Product category (CatalogElement)`.

This class defines the base methods, which are needed to instantiate a product node within the tree structure. Also some important properties are predefined.

The `OrderableProductNode` is a concrete implementation for a product from class `ProductNode`. There are more concrete implementations available for `ProductNode`.

**Predefined properties for ProductNode**

Each `ProductNode` has predefined properties. These methods are validated automated on constructor by the `validateProperties()` method.

|Identifier|Identifier eZ|Type|Description|
|--- |--- |--- |--- |
|name|ses_name|string|Product name|
|sku|ses_sku|string|Unique Stock Keeping Unit (SKU) of a product|
|manufacturerSku|ses_manufacturer_sku|string|Stock Keeping Unit (SKU) by a manufacturer of a product|
|ean|ses_ean|string|European Article Number (EAN) of a product|
|type|ses_product_type|string|Type of a product, e.g. vegetable|
|isOrderable||boolean|True, if a product is orderable|
|price|ses_unit_price|FieldInterface|Price of a product|
|customerPrice||PriceField (FieldInterface)|Customer price of a product which might be generated from a PriceProvider|
|scaledPrices||ArrayField|An array with scaled prices and parameters to determine which scale price should be applied|
|stock|ses_stock_numeric|FieldInterface|Returns the stock of  a product|
|subtitle|ses_subtitle|TextBlockField (FieldInterface)||
|shortDescription|ses_short_description|TextBlockField (FieldInterface)|Short description of a product|
|longDescription|ses_long_description|TextBlockField (FieldInterface)|Long description of a product|
|specifications|ses_specifications|FieldInterface[]|A list of specifications of a product|
|imageList|ses_image_1 .. ses_image_4|ImageField[] (FieldInterface[])|A list of images|
|minOrderQuantity|ses_min_order_quantity|float|Minimal order quantity of the product|
|maxOrderQuantity|ses_max_order_quantity|float|Maximal order quantity of the product|
|allowedQuantity||string|Regex that indicates the allowed quantity</br>this regex can be evaluated using the preg_match() function by some processes to check if the given quantity corresponds to the allowed quantity</br>values of ProductNodeConstants::ALLOWED_QUANTITY_* can be used</br>Possible constants:</br>ALLOWED_QUANTITY_INTEGER</br>ALLOWED_QUANTITY_UP_TO_ONE_DECIMAL_PLACE</br>ALLOWED_QUANTITY_UP_TO_TWO_DECIMAL_PLACES</br>ALLOWED_QUANTITY_MULTIPLE_DECIMAL_PLACES</br>Example for an individual expression: `'#^[0-9]+(\.|,)?[0-9]*$#'`</br>If not set the quantity can be int only!</br>Example:</br>`'allowedQuantity' =>  ProductNodeConstants::ALLOWED_QUANTITY_UP_TO_ONE_DECIMAL_PLACE,`|
|packagingUnit|ses_packaging_unit|float|Packaging unit of the product|
|unit|ses_unit|string|A unit of the product|
|vatCode|ses_vat_code|string|VAT code of the product. Needed to determine VAT percent|


For every property there is a public setter method.
