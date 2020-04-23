# Operation Basket

This class implements business logic for basket

## Methods

| method                                   | parameters                           | returns                                | purpose                    | operation identifier |
| ---------------------------------------- | ------------------------------------ | -------------------------------------- | -------------------------- | -------------------- |
| [addProducts](addproducts.md) | InputAddItemToBasket $operationInput | OutputAddItemToBasket $operationOutput | add products to the basket | basket.add\_products |
| [getBasket](getbasket.md)     | InputGetBasket $input                | OutputGetBasket $output                | returns current basket     | basket.get\_basket   |

!!! note

    Currently, the input value object of basket.get_basket contains a reference to the \Symfony\Component\HttpFoundation\Request service. Might get refactored after the architecture review.

## Service definition

services.business_layer.xml:

``` xml
<parameters>
    <parameter key="ses_eshop.basket.business_api.class">Silversolutions\Bundle\EshopBundle\Services\BusinessLayer\Operations\Basket</parameter>
</parameters>         

<!-- basket business component -->
<service id="ses_eshop.basket.business_api" class="%ses_eshop.basket.business_api.class%"
             parent="ses_eshop.business_api.base">
        <argument type="service" id="silver_basket.basket_service" />            
        <tag name="business_api_operation" alias="basket" />
</service> 
```
