# Wishlist and Stored Basket API

## Basket type

Wishlist and Stored Baskets are Baskets with a special type `wishlist` and `storedBasket`. 

When adding items no Events are thrown, therefore from the performance perspective it is quicker than adding items into basket. However, there is no data validation in the background, so it is allowed to mix different products. So the data validation, like checking the minimum order amount or checking the mixing of downloads with normal products, is done when adding those items into cart.

For wishlist and stored baskets only one template is used. The attributes that are shown can be handled via basket type.

``` php
const TYPE_STORED_BASKET = 'storedBasket';
const TYPE_WISH_LIST = 'wishList';
```
