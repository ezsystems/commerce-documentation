# Checkout Summary Form

!!! note "Namespace"

    `\Siso\Bundle\CheckoutBundle\Form\CheckoutSummary`

This document describes the HTML form for order summary in checkout process.

The order summary form derives from the `AbstractFormEntity`.

## Input Fields

|Name|Description|Assertations|
|--- |--- |--- |
|termsAndConditions|"Terms and conditions" Checkbox field|Boolean</br>NotBlank|
|comment|Optional user comment/remark|String</br>Length, max = 255|
|forceStep|True if user wants to force to next step with event errors|Boolean|

## Configuration

Please see [Configuration for Checkout Forms](../configuration_for_checkout_forms.md).

## Form Type

!!! note "Namespace"

    `\Siso\Bundle\CheckoutBundle\Form\Type\CheckoutSummaryType`

implements the setup for this form. In this class further definitions are implemented. 

This class is defined as a service in order to take advantage from other services, like TransService and to be able to read to configuration settings.

The service definition ID for this form is: `siso_checkout.form_entity.checkout_summary_type`

!!! note

    Please pay attention, that the scope of this service is set to 'prototype'. Against to default service behavior - a new instance of `Siso\Bundle\CheckoutBundle\Form\Type\CheckoutSummaryType` is created every time this service is called.

### Select values configuration

More configuration values are set in the config file checkout.yml.
