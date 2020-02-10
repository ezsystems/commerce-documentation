#  General logic in checkout 

For the general logic the [CheckoutController](CheckoutController_23560322.html) and the [AjaxCheckoutController](AjaxCheckoutController_23560323.html) are responsible.

## One Page Checkout - steps

1.  login
2.  invoice
3.  delivery
4.  shippingPayment
5.  summary

One Page Checkout enables the client to go through the process back and forth. All the steps are validated when ajax call is sent.

All the logic is moved into controllers which are responsible also for rendering.

## Checkout Activity Diagram - user cases in checkout process

![](attachments/23560637/23563812.jpg)

## Attachments:

![](images/icons/bullet_blue.gif) [Activity Checkout Fill Options.jpg](attachments/23560637/23563812.jpg) (image/jpeg)  
