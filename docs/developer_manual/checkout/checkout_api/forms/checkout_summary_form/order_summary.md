# Order summary

This chapter describes the general order summary.

Summary is the last step in the checkout process. Front end user, in order to end the process, needs to accept terms and conditions.

## Forms

Please see [Checkout Summary Form](checkout_summary_form.md).

## Templates

|                              |           |
| ---------------------------- | --------- |
| Main template                | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_summary.html.twig` |
| Sidebar template for summary | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/sidebar_summary.html.twig`  |

## Terms and conditions

In the form [Checkout Summary Form](Checkout-Summary-Form_23560350.html) there is a field `termsAndConditions` which is rendered as a checkbox.

The text for terms and conditions are stored as translatable text modules in the eZ backend (see `/Hidden-folder/Terms-Conditions-terms_conditions` in admin backend). This textmodule is defined as a label in the form type and will be fetched via TransService.

The front end user must accept the terms and conditions to complete the order.

Terms and conditions content is loaded via Ajax. It is possible to implement this functionality in the whole shop. Please take a look at this documentation page for a deeper understanding: [Pop-up window with external content](Pop-up-window-with-external-content_23560535.html)

## Screenshot

![](../../../../img/order_summary.png)
