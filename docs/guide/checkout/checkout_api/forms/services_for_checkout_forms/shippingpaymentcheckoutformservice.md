# ShippingPaymentCheckoutFormService

ShippingPaymentCheckoutFormService is a service that implements the logic for the CheckoutShippingPayment form.This service is assigned to the CheckoutShippingPayment form in the [configuration](../configuration_for_checkout_forms.md).

This service implements the [CheckoutFormServiceInterface](interfaces_for_checkout_services.md#checkoutformserviceinterface).

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Service\ShippingPaymentCheckoutFormService`

!!! note "Service ID"

    `siso_checkout.checkout_form.shipping_payment`

## Usage

**Example**

``` php
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
