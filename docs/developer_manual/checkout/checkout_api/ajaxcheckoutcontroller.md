# AjaxCheckoutController

The `AjaxCheckoutController` is the subcontroller responsible for the phalanx calls in the one-page checkout process.

!!! note "Namespace"

    `Siso\Bundle\CheckoutBundle\Controller\AjaxCheckoutController`

## Steps

AjaxCheckoutController first calls validateStepAction for calculating the current possible step depending on :

1. form validation
1. submitted data (checkboxes)
1. customer data
1. basket (eg. calling \#summary when no delivery address is filled will cause redirection to delivery step)

### How the steps are handled?

There is an interface defined for the steps handling.

`Siso/Bundle/CheckoutBundle/Api/CheckoutStepServiceInterface.php`

``` php
/**
 * Returns the next valid checkout step for $currentStep.
 * The $requestedStep is preferred, but could be skipped due to some
 * implemented validation rules.
 *
 * The result must be one of CheckoutSources::STEP_* constants
 * @see \Siso\Bundle\CheckoutBundle\Model\CheckoutSources
 *
 * @param string $currentStep   One of CheckoutSources::STEP_* constants
 * @param string $requestedStep One of CheckoutSources::STEP_* constants
 * @param string $userStatus    One of CheckoutSources::USER_* constants
 * @param Basket $basket
 * @return null|string  One of CheckoutSources::STEP_* constants
 */
public function getNextValidStep($currentStep, $requestedStep, $userStatus, Basket $basket);
```

`Siso/Bundle/CheckoutBundle/Service/DefaultCheckoutStepService.php`

Default implementation of this interface is *DefaultCheckoutStepService,* that ensures that user is forwarded to the correct step and all necessary data is stored in the basket. The  goal is to check if user is able to see the requested step. If he has no right to it, he will be forwarded to the last possible step.

The checkout steps are defined here.

``` php
public static final function getAllCheckoutSteps()
{
    return array(
        CheckoutSources::STEP_LOGIN,
        CheckoutSources::STEP_INVOICE,
        CheckoutSources::STEP_DELIVERY,
        CheckoutSources::STEP_SHIPPING_PAYMENT,
        CheckoutSources::STEP_SUMMARY
    );
}
```

`validateStepAction` makes usage of this service to determine the correct step.

``` php
/** @var CheckoutStepServiceInterface $checkoutStepService */
$checkoutStepService = $this->get('siso_checkout.default_checkout_step_service');

$requestedStep = isset($data[0]['step']) && $data[0]['step'] ? $data[0]['step'] : $currentStep;
$currentStep = $checkoutStepService->getNextValidStep($currentStep, $requestedStep, $userStatus, $basket);
```

If you want to skip a step in checkout process, you will need to override the *DefaultCheckoutStepService*. When skipping steps, please make sure that you store all required data in the basket.

`DefaultCheckoutStepService` service definition:

``` xml
<parameter key="siso_checkout.default_checkout_step_service.class">Siso\Bundle\CheckoutBundle\Service\DefaultCheckoutStepService</parameter>

<service id="siso_checkout.default_checkout_step_service" class="%siso_checkout.default_checkout_step_service.class%">
    <argument type="service" id="ses.customer_profile_data.ez_erp" />
    <argument type="service" id="silver_basket.basket_service" />
</service>
```

In the `validateStepAction` Checkout Events are thrown, that allows you to interrupt the checkout process if required.

#### `validateLogin`

|||||
|---|---|---|---|
|form valid||||
|submitted data||||
|customer data|anonymous|customer without no|customer with no|
|current step|login|delivery|shippingPayment|


#### `validateInvoice`

form value: true

||||
|---|---|---|
|submitted data|invoiceAsDelivery checked|invoiceAsDelivery not checked|
|customer data|||
|current step|shippingPayment|delivery|

form value: false

||||
|---|---|---|
|submitted data|-|-|
|customer data|	customer without no|else|
|current step|delivery|invoice|

#### `validateDelivery`

||||
|---|---|---|
|form valid|true|false|
|submitted data|-|-|
|customer data|||
|current step|shippingPayment|delivery|

#### `validateShippingPayment`

||||
|---|---|---|
|form valid|true|false|
|submitted data|-|-|
|customer data|||
|current step|summary|shippingPayment|

#### `validateSummary`

||||
|---|---|---|
|form valid|true|false|
|submitted data|||
|customer data|||
|current step|summary|summary|

#### Steps for phalanx actions on radio buttons on delivery address

|||||
|--- |--- |--- |--- |
|action name|getExistingDelivery|getEmptyDelivery|getDeliveryFromInvoice|
|Current step|delivery|delivery|delivery|

## Logic

1. validateLogin

    It displays login form, possible options for registration and option to order as a guest. 
     
2. validateInvoice

    On success it stores inovice address in basket. If a checkbox "invoiceAsDelivery" was set then it stores also delivery address in the basket.  

3. validateDelivery

    The form is created based on configuration and determines which addressStatus radio button option should be chosen. There are different scenario for anonymous, user without and with Customer Number.   
    When user have no default delivery address, option with displaying "existing" addresses is disabled.  
    On success it stores delivery address in basket and also if saveAddress was checked it stores the address in the CustomerProfileData.  

4. validateShippingPayment

    On success it stores shipping/payment method in basket.  

5. validateSummary

    On success:
      
    - it stores data in basket  
    - copies the basket with state "Confirmed"  
    - redirects to order confirmation

    In this step, the total gross amount may be rounded to 2 decimal digits. This is because the amount is used to process the payment later on and nearly all payment transaction don't allow values with more than 2 decimal digits. It depends to some configuration:
    
    `#checkout.ymlsiso_checkout.default.payment.round_totals: true`
    
    The default is `true`. If the rounding should be avoided, it can be set to `false`.

6. getExistingDelivery
 
    Triggered with delivery addressStatus option. It looks for customer delivery Parties and files the form with them. Sets the default address if no data set. Returns delivery template form.  

7. getEmptyDelivery

    Triggered with delivery addressStatus option. Returns empty delivery template form.
    
8. getDeliveryFromInvoice

    Triggered with delivery addressStatus option. Returns delivery template form based on invoice form previously filled.
      
9. validateStep

    Validates the step for which the request was made and return available current step. This prevents from going into steps in which user cannot be.   
    Additionally, if user is logged with customer number, the request to NAV will take place in order to get latest invoice address. The reason for that is to prevent showing incorrect address in the basket for the user.

### Overriding the controller

To override specific or all acions of this controller, you will need to create a new controller which extends the AjaxCheckoutController (for example NewAjaxCheckoutController). In the new controller you can reimplementend the actions (For example validateSummary). In order to activate the new Controller for Ajax requests, you will need to register the new controller class as the controller for Ajax requests with the type 'checkout'. This is done by a parameter:

``` yaml
parameters:
    siso_eshop.ajax_controller.checkout: "YourBundle:NewAjaxCheckout"
```

For more information please have a look at [Ajax (Phalanx)](../../../cookbook/ajax_phalanx.md).
