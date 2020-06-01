# Wishlist and stored basket templates

## Template list

The templates are located in `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views`.

|Path|Description|
|--- |--- |
|`Basket/stored_baskets_list.html.twig`|List of all stored baskets|
|`Basket/show_stored_basket.html.twig`|Entry page for wishlist and stored baskets. Based on the basket type it loads one of the templates listed below|
|`Basket/show_stored_basket_part.html.twig`|Partial page responsible for rendering stored basket page|
|`Basket/show_wishlist_part.html.twig`|Partial page responsible for rendering wishlist page|
|`Basket/messages.html.twig`|Template with success/error/notice messages for baskets|

## Twig functions

- `get_stored_baskets` - returns stored baskets for current user

``` html+twig
{% set storedBaskets = get_stored_baskets() %}
{% if storedBaskets|default is not empty %}
```
