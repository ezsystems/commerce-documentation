# Checkout Shipping Payment Form

## Model Class

This class extends the `AbstractFormEntity` and implements the `CheckoutAddressInterface`.

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Form\CheckoutShippingPayment`

## Input Fields

|Name|Description|Assertations|
|--- |--- |--- |
|shippingMethod|method of the shipping|String</br>Not blank|
|paymentMethod|method of the payment|String</br>Not blank|
|forceStep|True if user wants to force to next step with event errors|Boolean|

## Configuration

The parameters are set in the [Configuration for Checkout Forms](configuration_for_checkout_forms.md).

## Form Type

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Form\Type\CheckoutShippingPaymentType`

implements the setup for this form. In this class further definitions are implemented. 

This class is defined as a service in order to take advantage from TransService.

The service definition ID for this form is: `siso_checkout.form_entity.checkout_shipping_payment_type`

!!! note

    Please pay attention, that the scope of this service is set to 'prototype'. Against to default service behavior - a new instance of ` Siso\Bundle\CheckoutBundle\Form\Type\CheckoutShippingPaymentType` is created every time this service is called.

## Templates

|               |           |
| ------------- | --------- |
| Main template | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_shipping_payment.html.twig` |

### Select values configuration

More configuration values are set in the [Configuration for Checkout Forms](configuration_for_checkout_forms.md).
