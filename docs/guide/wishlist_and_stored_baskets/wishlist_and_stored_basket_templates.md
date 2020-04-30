# Wishlist and Stored Basket templates

### Templates list

Default path:

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views`

|Path|Description|
|--- |--- |
|Basket/stored_baskets_list.html.twig|list of all stored baskets|
|Basket/show_stored_basket.html.twig|entry page for wishlist and stored baskets. Based on the basket type it loads one of the templates listed below|
|Basket/show_stored_basket_part.html.twig|partial page responsible for rendering stored basket page|
|Basket/show_wishlist_part.html.twig|partial page responsible for rendering wishlist page|
|Basket/messages.html.twig|template with success/error/notice messages for baskets|

### Related custom Twig modifiers/functions/etc/

#### Twig functions

|Twig function|Description|Usage|
|--- |--- |--- |
|get_stored_baskets|returns stored baskets for current user|{% set storedBaskets = get_stored_baskets() %}</br>{% if storedBaskets|default is not empty %}</br>...|
