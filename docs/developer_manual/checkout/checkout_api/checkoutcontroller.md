#  CheckoutController 

The CheckoutController is the entry place in the checkout process.

Namespace

Siso\\Bundle\\CheckoutBundle\\Controller\\CheckoutController

# IndexAction

Before users enter the checkout process (index action), [Checkout Events](Checkout-Events_23560944.html) are thrown, that allows you to interupt the checkout process if required.

URL

/checkout

When client calls indexAction it triggers **validateStepAction.** Depending in which step customer can be, it forwards the call to that AjaxController method. 
