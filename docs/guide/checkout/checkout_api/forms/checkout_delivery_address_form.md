# Checkout Delivery Address Form

## Model class

This class extends the `AbstractFormEntity` and implements the `CheckoutAddressInterface`

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Form\CheckoutDeliveryAddress`

This document describes the form for choosing delivery address in checkout process.

## Fields

|Name|Description|Assertations|
|--- |--- |--- |
|addressStatus|status of delivery address||
|company|user name or company|Not Blank</br>min = 2</br>max = 30|
|companySecond|user second name or company|min = 2</br>max = 30|
|street|street of the user|Not Blank</br>min = 2</br>max = 30|
|addressSecond|optional second address of the user|min = 2</br>max = 30|
|zip|zip number of the user|Not Blank (excluding Ireland)</br>min = 3</br>max = 20</br>Numeric|
|city|city of the user|Not Blank</br>min = 2</br>max = 30|
|country|country of the user|Not Blank|
|county|county of the user|min = 2</br>max = 30|
|phone|phone number of the user|SesAssert\Phone|
|email|email address of the user|SesAssert\Email|
|saveAddress|true if user wants to store this address</br>in his address list|boolean|
|partyId|Id of the party ID in ERP system|string|
|forceStep|True if user wants to force to next step with event errors|Boolean|

## Form Type

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Form\Type\CheckoutDeliveryAddressType`

implements the setup for this form. In this class further definitions are implemented.

This class is defined as a service in order to take advantage from other services, like TransService and to be able to read the configuration settings.

The service definition ID for this form is: `siso_checkout.form_entity.checkout_delivery_address_type`

!!! note

    Please pay attention, that the scope of this service is set to 'prototype'. Against to default service behavior - a new instance of `Siso\Bundle\CheckoutBundle\Form\Type\``CheckoutDeliveryAddressType` is created every time this service is called.

## Configuration

see [Checkout Forms Configuration](configuration_for_checkout_forms.md).

## Templates

|                              |        |
| ---------------------------- | ------ |
| Main template                | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_delivery_address.html.twig` |
| Sidebar template for invoice | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/sidebar_delivery_address.html.twig`  |

## General logic

Error rendering macro 'excerpt-include' : No link could be created for 'INTERNAL:Detail concept delivery address box'.

### Exceptions in validation process for delivery

In some cases we need to suppress the form validation

- if user has customer number and uses invoice as delivery
- if user has customer number and uses address from a list. In these cases the data is coming from ERP and user can not change it.
