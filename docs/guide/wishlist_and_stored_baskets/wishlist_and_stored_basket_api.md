# Wishlist and stored basket API

## Basket type

Wishlists and stored baskets are baskets with a special type `wishlist` or `storedBasket`. 

When adding items no events are thrown, so adding to wishlists or stored baskets is quicker than adding items to a basket.
However, there is no data validation in the background, so you can mix different products.

Data validation, such as checking the minimum order amount or checking the mixing of downloads with normal products,
is done when adding those items into a cart.

One template is used for both wishlists and stored baskets. The attributes that are shown can be handled through the basket type:

``` php
const TYPE_STORED_BASKET = 'storedBasket';
const TYPE_WISH_LIST = 'wishList';
```
