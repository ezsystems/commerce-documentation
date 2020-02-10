#  ShippingPaymentCheckoutFormService 

ShippingPaymentCheckoutFormService is a service that implements the logic for the CheckoutShippingPayment form.This service is assigned to the CheckoutShippingPayment form in the [configuration](http://confluence.ng.silverproducts.de/display/EX/Configuration+for+Checkout+Forms).

This service implements the [CheckoutFormServiceInterface](http://confluence.ng.silverproducts.de/display/EX/Interfaces+for+checkout+services).

Namespace

    Siso\Bundle\CheckoutBundle\Service\ShippingPaymentCheckoutFormService

Service ID

    siso_checkout.checkout_form.shipping_payment 

## Usage

**Example**

``` 
$formService = $this->container->get('siso_checkout.checkout_form.shipping_payment');
/** @var BasketService $basketService */
$basketService = $this->container->get('silver_basket.basket_service');
$basket = $basketService->getBasket($request);
 
$form = $this->handleForm($request, $data, $basket);
if ($form->isValid()) {
    if ($form->getViewData()->hasChanged()){
       $formService->storeFormDataInBasket($form->getViewData(), $basket);
    }
}
```
