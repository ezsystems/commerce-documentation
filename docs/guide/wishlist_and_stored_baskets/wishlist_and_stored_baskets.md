# Wishlist and stored baskets

The customer can save their favorite products in a list and easily access them or add them to the cart.

Two different types of lists are available:

- [wishlists](wishlist_and_stored_basket_wishlist.md)
- [stored baskets](wishlist_and_stored_basket_stored_baskets.md) - custom named lists

This feature is available only for logged-in customers.

The difference between a wishlist and a stored basket is that there is only one wishlist per user,
but there can be many stored baskets with different names for one user. 

A stored basket can also contain quantity of products and prices.

The stored basket feature can be deactivated in configuration:

``` yaml
twig:
    globals:
        stored_baskets_active: false
```
