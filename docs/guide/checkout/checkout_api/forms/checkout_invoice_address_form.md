# Checkout Invoice Address Form

## Model Class

This class extends the `AbstractFormEntity` and implements the `CheckoutAddressInterface`.

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Form\CheckoutInvoiceAddress`

## Input Fields

|Name|Description|Assertations|
|--- |--- |--- |
|company|name or company of the user|Not blank</br>min = 2</br>max = 30|
|companySecond|second name or company of the user|min = 2</br>max = 30|
|street|street of the company|String</br>Not blank</br>min = 2</br>max = 30|
|addressSecond|second address of the user|String</br>min = 2</br>max = 30|
|zip|zip of the user|String</br>Not blank (excluding Ireland)</br>min=2</br>max=30|
|city|city of the user|String</br>Not blank</br>min=2</br>max=30|
|country|country of the user|String</br>Not blank|
|county|county of the user|String</br>min=2</br>max=30|
|phone|phone number of the user|SesAssert\Phone|
|email|email of the user|Not blank</br>SesAssert\Email|
|invoiceSameAsDelivery|True if user wants to use this address as delivery address|Boolean|
|forceStep|True if user wants to force to next step with event errors|Boolean|

## Configuration

The parameters are set in the [Configuration for Checkout Forms](configuration_for_checkout_forms.md).

## Form Type

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Form\Type\CheckoutInvoiceAddressType`

implements the setup for this form. In this class further definitions are implemented. 

This class is defined as a service in order to take advantage from other services, like TransService and to be able to read the configuration settings.

The service definition ID for this form is: `siso_checkout.form_entity.checkout_invoice_address_type`

!!! note 

    Please pay attention, that the scope of this service is set to 'prototype'. Against to default service behavior - a new instance of `Siso\Bundle\CheckoutBundle\Form\Type\CheckoutInvoiceAddressType` is created every time this service is called.

## Templates

|                              |        |
| ---------------------------- | -------|
| Main template                | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_invoice_address.html.twig` |
| Sidebar template for invoice | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/sidebar_invoice_address.html.twig`  |

### Exceptions in validation process for invoice

In some cases we need to suppress the form validation

- if user has customer number
