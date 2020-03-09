# Business API Invocation Service

This service is the access point to the **Business API** and defined by a service with the id **ses\_eshop.business\_api.invocation**. Since this service acts only as a proxy, the business logic itself is outsourced into different *operation services*.

## Call Business API operation service

To call Business API *operation service*, the call method of BusinessApi invocation service must be used.

|method|parameters|returns|
|--- |--- |--- |
|call()|string $operationIdentifier</br>ValueObject $operationInput|ValueObject $operationOutput|

### How to create $operationIdentifier?

The identifier of the Business API *operation service* consists of two parts separated by a dot **.**

1.  The first part is the alias of the operation service, that can be found in the configuration: services.business\_layer.xml
1.  The second part is the operation method, that should be invoked. The method name must have minor letters separated by underscore instead of the method's camelCase notation ( get\_basket =\> getBasket() ).

**Examples:**

``` 
basket.get_basket
basket.add_products
catalog.load_products
```

#### Example

This is an example how to use the BusinessApi *Basket Operation Service*.

``` php
use Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\InputValueObjects\GetBasket as InputGetBasket;
use Silversolutions\Bundle\EshopBundle\Entities\BusinessLayer\OutputValueObjects\GetBasket as OutputGetBasket;

/** @var InputGetBasket $input */
$input = new InputGetBasket(array('request' => $request));

/** @var OutputGetBasket $output */
$output = $this->get('ses_eshop.business_api.invocation')->call('basket.get_basket', $input);
```

#### Implementing a BusinessApi Operation

In order extend the Business API, you need to create a new service class. The operations of that class contain the business logic respectively and are stored here: *silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Services/BusinessLayer/Operations/\**

Each operation, e.g. Basket, is a simple symfony2 service. While defining the service, please make sure you add a **tag** to the service definition like this:

``` 
<service id="ses_eshop.basket.business_api" class="%ses_eshop.basket.business_api.class%" parent="ses_eshop.business_api.base">
   <argument type="service" id="silver_basket.basket_service" />
   <tag name="business_api_operation" alias="basket" />
</service>
```

!!! note

    The tag **name** must always be `business_api_operation`.

    The attribute **alias** defines the above mentioned first part in the operation identifier.

    Please derive your new service from the [common parent class BaseOperation](BaseOperation_23560335.html) in order to get access to commonly needed dependencies (e.g. logging and translations).

#### Overriding an existing BusinessApi class

If you want to override a particular API service you need to create a new service class, which extends the orginal. Within that class it is possible to override individual methods or add new methods. Then you need to redefine the service in the Symfony configuration with the new class and the same tag values (name and alias).

Example

**Service class**

``` php
namespace Example\Bundle\ExtensionBundle\Services\BusinessLayer\Operations;

class NewBasketApi extends Silversolutions\Bundle\EshopBundle\Services\BusinessLayer\Operations\Basket
{
    public function newBasketOperation(InputBasketOperation $operationInput) {
        // process something
        // ..
        return new OutputBasketOperation(array('resultAttribute' => $result));
    }
    
    public function addProducts(InputAddItemToBasket $operationInput)
    {
        // override method logic
        // ..
        return new OutputAddItemToBasket(array('basket' => $basketObject));
    }
}
```

There are two ways to redefine the service configuration:

If the original service was defined with a configuration parameter (% notation) in its class attribute, then you just need to redefine that parameter with your created fully qualified class name.

``` yaml
parameters:
    ses_eshop.basket.business_api.class: 'Example\Bundle\ExtensionBundle\Services\BusinessLayer\Operations\NewBasketApi'
```

The other option is to completely redefine the original service with the new fully qualified class name.

``` xml
<service id="ses_eshop.basket.business_api" class="Example\Bundle\ExtensionBundle\Services\BusinessLayer\Operations\NewBasketApi" parent="ses_eshop.business_api.base">
   <argument type="service" id="silver_basket.basket_service" />
   <tag name="business_api_operation" alias="basket" />
</service>
```
