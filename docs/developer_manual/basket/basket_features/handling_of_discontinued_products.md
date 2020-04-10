# Handling of discontinued products

A Listener can check if a product is not sold anymore (discontinued) and the user might have to be informed that this product is not available any more. This feature can be disabled (discontinued_products_listener_active).

The listener will check if the current stock is equal or higher than the quantity the customer wants to order. In this case the order will be allowed. The optional config setting  "discontinued_products_listener_consider_packaging_unit" might allow to skip the packaging unit in order to sell the remaining products even if remaining stock does not fit to the packing unit rule (e.g. packing unit = 10 pieces but 9 are left in stock). The listener will reduce the quantity in the order to the number of products which are in stock. 

``` yaml
siso_basket.default.discontinued_products_listener_active: true
siso_basket.default.discontinued_products_listener_consider_packaging_unit: true
```
