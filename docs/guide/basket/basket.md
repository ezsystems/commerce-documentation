# Basket

![](../img/basket_1.jpg)

eZ Commerce provides a flexible basket system. A basket is identified by: 

- session ID (if the user is not logged in)
- user ID (if the user is logged in)

Doctrine is used to store the basket in the database.  

The basket can have one of the following types:

- `basket`
- `quickOrder`
- `storedBasket`, stored with a user-set name
- `wishList`
- `comparison`

A standard basket (type `basket`) may use different states during the checkout process. After the order has been sent, the basket will get the special state `ordered`.

## Basket structure

The basket enables storing project-specific information in several places:

- Basket header
- Basket lines
- Basket Parties

### Basket header

Additional fields can be placed in the basket header (`dataMap` Field).

These can be simple fields or complex data as well.

### Basket lines

Basket lines contains info such as:

- SKU
- variant code
- quantity
- prices
- a remark or additional data per basket line. See [Additional data in the basket line](basket_features/additional_data_in_the_basket_line.md)

### Basket parties

Basket parties include:

- BuyerParty - address of the customer (usually this address is the same as the InvoiceParty)
- InvoiceParty - address for the invoice from the checkout or customer profile
- DeliveryParty - delivery address given by the customer

## Baskets in multishop

For each shop a `shop_id` can be configured. The `shop_id` will be stored in the basket.

## Additional costs

Additional costs can be stored in the basket to implement all kinds of additional costs or discounts, such as:

- shipping costs
- costs for packaging
- costs for payment
- discount for vouchers
- general discounts for total order

## Customer remarks

The customer can provide a general remark for the basket in the checkout.

## Handling of discontinued products

See [Discontinued products](basket_features/discontinued_products.md)

## Performance improvement

The BasketLine stores a serialized version of the product in the basket line.
Configuration controls which Fields from the product are stored.

## Price calculation

A price engine calculates the prices and provides up-to-date information about stock.
The basket contains a flag which controls if a recalculation has to be done (timeout).

## Merging of baskets

After a login eZ Commerce merges the products from the existing basket (filled as an anonymous user) and a basket which is already stored for the given user id of the user.

If an SKU is already present in a basket, a new line is created in the basket of the user.

## Shared baskets

A basket can be shared if a user logs in in different browsers (default) or bound to session only.

In case a basket shall be not shared in case the same user logs in twice a setting can be adapted (default: `false`)

`ses_basket.default.basketBySessionOnly: true`
