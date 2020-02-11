#  Basket - Eventlistener in the standard 

eZ Commerce offers two standard eventlisteners.

## StandardBasketListener

The basket comes with a standard event listener which provides a logic for standard rules.

 The standard listener provides the following checks:

  - check if the quantity is valid for a catalogElement
  - check for a minimum quantity 
  - check for a maximum quantity
  - check the packaging unit
  - purges the basket cache after changes in the basket

#### Allowed quantity

The allowed quantity is stored in catalogElement as a regex - *$allowedQuantity* - and is evaluated by a *preg\_match()* function.

If the [catalog factory](CMS-dataprovider_23560465.html) did not set the $allowedQuantity attribute in the catalog element, only int quantity is valid\!

#### Min and Max quantity

The MIN and MAX values are stored in EshopBundle/Resources/config/basket.yml as a *default*, if not set in catalogElement as *$minOrderQuantity* and *$maxOrderQuantity*.

``` 
parameters:
    silver_basket.basketline_quantity_max: 99
    silver_basket.basketline_quantity_min: 1
```

#### Packaging unit

The packaging unit is stored in catalogElement as *packagingUnit*. If the requested quantity does not correspondent the packaging unit, the quantity is increased to the next possible quantity that will corresponding the packaging unit.

## DiscontinuedProductsListener

This listener handles discontinued products. It is active when it's enabled in configuration:

How to enable/disable this listener?

    parameters:

``` 
 #here you can enable/disable the DiscontinuedProductsListener
 #values: true or false
 siso_basket.default.discontinued_products_listener_active: true
```

#### When the catalog element is recognised as discontinued?

This listener is only active for products that are marked as 'discontinued'

``` 
$catalogElement.dataMap.discontinued = new TextLineField(array('text' => 1));
```

### Logic

There are **3 possibilities** when listener checks stock information:

  - **stock is not available** - error message is displayed that there is no information about the availability for product, but it is discontinued.
  - **stock is 0** - item is removed from the basket and error message is displayed that the item is discontinued and not available anymore.
  - **ordered quantity is bigger than stock** - the quantity is modified in the basket line and notice message is set in the basket.
