# Basket features

The basket will offer a variety of features. Some of the features will be described in detail on separate pages.

## Overview

### Store different kind of baskets

The basket can have several types. silver.eShop provides

- basket
- quickOrder
- storedBasket
- wishList
- comparison

### A user can store a basket with a name

Is used e.g. for named basket of type "storedBasket"

### Differentiate baskets between shops (Multishop)
Advanced version only

For each shop a shop_id can be configured. The shop_id will be stored in the basket

### Stores the BuyerParty

The address of the customer (usually this address is the same as the InvoiceParty)

### Stores the InvoiceParty

Address for the invoice from the checkout or customer profile

### Stores the DeliveryParty

The delivery address given by the customer

### Basket header - add fields

Additional fields can be placed in the basket header (dataMap Field)

This can be simple fields or complex data as well

### Additional costs can be stored in the basket to implement all kind of additional costs or discounts

Additional costs or discounts such as

- shipping costs
- costs for packaging
- costs for payment
- discount for vouchers
- general discounts for total order

### A general remark for the basket

provided by the customer in the checkout

### 1..n basket lines

contains info such as

- sku
- variant code
- quantity
- prices
- a remark or additional data per basketline. See [Additional data in the basket line](additional_data_in_the_basket_line.md)

### Handling of discontinued products

See [Handling of discontinued products](handling_of_discontinued_products.md)

### Performance improvement

The basketline stores a serialized version of the product in the basket line.
A configuration controls which fields from the product shall be stored

### Calculation of prices if required

A Price Engine will be involved. It will calculate the prices and provide up-to-date information about stock.
The basket contains a flag which controls if a recalculation has to be done (timeout).

### Merging of baskets

After a login silver.eShop merges the products from the existing basket (filled as a anonymous user) and a basket which is already stored for the given user id of the user.

If a sku is already present in a basket a new line will be created in the basket of the user.

### Basket can be shared if a user logs in in different browsers (default) or bound to session only

In case a basket shall be not shared in case the same user logs in twice a setting can be adapted (default: false)

`ses_basket.default.basketBySessionOnly: true`
