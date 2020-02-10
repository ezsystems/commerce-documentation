#  Configuration for Checkout Forms 

In order to be able to override the forms and form logic in the a specific project, there is a generic configuration for checkout forms.

## Configuration class

Namespace

`Siso\Bundle\CheckoutBundle\Model\FormConfig`

## Usage of this configuration

The checkout forms are configured using the file `vendor/silversolutions/silver.e-shop/src/Siso/Bundle/CheckoutBundle/Resources/config/checkout.yml` .

**\~/vendor/silversolutions/silver.e-shop/src/Siso/Bundle/CheckoutBundle/Resources/config/checkout.yml**

``` 
parameters:
    checkoutForms:
        invoice:
            modelClass: Siso\Bundle\CheckoutBundle\Form\CheckoutInvoiceAddress
            typeService: siso_checkout.form_entity.checkout_invoice_address_type
            #typeClass:
            template: SilversolutionsEshopBundle:Checkout:checkout_invoice_address.html.twig
            templateSidebar: SilversolutionsEshopBundle:Checkout:sidebar_invoice_address.html.twig
            invalidMessage: error_message_checkout_invoice_address
            validMessage: success_message_checkout_invoice_address
            service: siso_checkout.checkout_form.invoice_address
        delivery:
            modelClass: Siso\Bundle\CheckoutBundle\Form\CheckoutDeliveryAddress
            typeService: siso_checkout.form_entity.checkout_delivery_address_type
            #typeClass:
            template: SilversolutionsEshopBundle:Checkout:checkout_delivery_address.html.twig
            templateSidebar: SilversolutionsEshopBundle:Checkout:sidebar_delivery_address.html.twig
            invalidMessage: error_message_checkout_delivery_address
            validMessage: success_message_checkout_delivery_address
            service: siso_checkout.checkout_form.delivery_address
        shippingPayment:
            modelClass: Siso\Bundle\CheckoutBundle\Form\CheckoutShippingPayment
            typeService: siso_checkout.form_entity.checkout_shipping_payment_type
            #typeClass:
            template: SilversolutionsEshopBundle:Checkout:checkout_shipping_payment.html.twig
            invalidMessage: error_message_checkout_shipping_payment
            validMessage: success_message_checkout_shipping_payment
            service: siso_checkout.checkout_form.shipping_payment
        summary:
            modelClass: Siso\Bundle\CheckoutBundle\Form\CheckoutSummary
            typeService: siso_checkout.form_entity.checkout_summary_type
            #typeClass:
            template: SilversolutionsEshopBundle:Checkout:checkout_summary.html.twig
            templateSidebar: SilversolutionsEshopBundle:Checkout:sidebar_summary.html.twig
            invalidMessage: error_message_checkout_summary
            validMessage: success_message_checkout_summary
            service: siso_checkout.checkout_form.summary
```

You are able to fully modify this configuration just by setting different values. It enables you to override e.g. the Form Type, Form Service or templates, or anything you need to change.

Please keep in mind that if you change the form model class, you should also override the form type, templates and also the form service cause the logic might changed.

## Other configuration values

Here the choices for the forms and preferred choices are set. The choices for the delivery address depends on the user status.

**\~/vendor/silversolutions/silver.e-shop/src/Siso/Bundle/CheckoutBundle/Resources/config/checkout.yml**

``` 
parameters:
    ses_forms.checkout_values:
        deliveryAddressStatusCustomerNr:
            sameAsInvoice: use_invoice_as_delivery
            new: new_delivery
            existing: existing_delivery
        deliveryAddressStatusAnonymous:
            sameAsInvoice: use_invoice_as_delivery
            new: new_delivery
        deliveryAddressStatusNoCustomerNr:
            sameAsInvoice: use_invoice_as_delivery
            new: new_delivery
        shippingMethods:
            standardMail: standard_mail
            mail: mail
            expressDelivery: express_delivery
        paymentMethods:
            paypal: paypal
            invoice: invoice
            creditCard: credit_card

    ses_forms.checkout_preferred_choices:
        preferred_delivery_address_status_no_customer_nr: new
        preferred_delivery_address_status_anonymous: new
        preferred_delivery_address_status_customer_nr: sameAsInvoice
        preferred_shipping_method: standardMail
        preferred_payment_method: creditCard
```

The shipping and payment methods configuration can be also overriden by implementing an [Pre Form Checkout Event](Checkout-Events_23560944.html) that will store the values in the dataMap.

Important

Please keep in mind that the **value** of the preferred choice and the **index** of the choice must match\!

Example:

ses\_forms.checkout\_values:

    deliveryAddressStatusCustomerNr:

        **sameAsInvoice**: use\_invoice\_as\_delivery

        new: new\_delivery existing:

        existing\_delivery

ses\_forms.checkout\_preferred\_choices:

    preferred\_delivery\_address\_status\_customer\_nr: **sameAsInvoice**

``` 
```
