# CheckoutController

The CheckoutController is the entry place in the checkout process.

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Controller\CheckoutController`

## IndexAction

Before users enter the checkout process (index action), [Checkout Events](checkout_events.md) are thrown, that allows you to interrupt the checkout process if required.

!!! tip "URL"

    `/checkout`

!!! caution

    When client calls indexAction it triggers `validateStepAction`. Depending in which step customer can be, it forwards the call to that AjaxController method. 
