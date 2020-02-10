#  Product quantity validation 

The quantity of added products is validate on different places.

  - **In the BasketController**

There is a help function *validateQuantity*(), that checks if the given quantity (string - because it comes from a form) correspondent the rules. (e.g. for 'float' quantity also comma is allowed). If it does the quantity is modified to a valid float value.

If it doesn´t the quantity is just returned, but not changed - it will be validate in StandardBasketListener

  - *addAction*() - after the quantity was validated, the addAction checks, if it is 0. Than it is changed to 1.
  - *updateAction*() - after the quantity was validated, the addAction checks, if it is 0. Than the deleteAction() is called and the product will be removed from basket.

  - **StandardBasketListener**
      - Checks if the given quantity is valid (e.g. negative numbers or strings are not allowed). If it is not valid, the action is not allowed (STATUS\_FAILED).
      - Checks if the given quantity is more than MAX. Than it is set to MAX.
      - Checks if the given quantity is less than MIN. Than it is set to MIN.  

    **standard config in vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/basket.yml**

    ``` 
    silver_basket.basketline_quantity_max: 1000000
    silver_basket.basketline_quantity_min: 1
    ```
