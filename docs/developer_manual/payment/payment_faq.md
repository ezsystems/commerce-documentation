# Payment FAQ

## How do I implement a new payment provider plugin?

Please refer to [Building new payment plugins](payment_cookbook/building_new_payment_plugins.md).

## How can I activate the payment options in checkout process?

Individual payment options / payment methods are implemented in symfony bundles. As soon as they are registered in the kernel, they are displayed in the checkout payment form. The respective options are registered to the checkout form by a DIC compiler pass und must fulfill some requirements:

- The bundle must define a form type service
- The form type must define the tag: `payment.method_form_type`
- The form type must define the tag: `form.type` with an alias attribute, that is used as text in the payment choice.
- Example:

``` xml
<service id="payment.form.telecash_connect_type"
         class="%payment.form.telecash_connect_type.class%">
    <tag name="payment.method_form_type" />
    <tag name="form.type" alias="telecash_connect" />
</service>
```

The compiler pass is defined in the checkout bundle: `\Siso\Bundle\CheckoutBundle\DependencyInjection\Compiler\PaymentMethodsPass`

## How can I influence / customize the payment options, displayed on the box payment options (e.g. Hide some if products are not available)?

In order to change the shipping-payment form, the form type service itself has to be replaced / rewritten. The form type for the options choice is implemented in the checkout bundle: `\Siso\Bundle\CheckoutBundle\Form\Type\CheckoutShippingPaymentType`

The methods `CheckoutShippingPaymentType::buildForm()` and `CheckoutShippingPaymentType::setDefaultOptions()` are called to assemble the HTML form.

Please check the [Configuration for the Checkout Forms](../checkout/checkout_api/forms/configuration_for_checkout_forms.md).

Please refer to [symfony's form documention](http://symfony.com/doc/current/book/forms.html) for more details.

## Due to a restriction in JmsPaymentBundle there is a restriction that an order exceeding 99999.99999 causes an error message. How do I extend it?

There is an official workaround for the JMSPaymentCoreBundle: <http://jmspaymentcorebundle.readthedocs.io/en/latest/guides/overriding_entity_mapping.html>

You will need to override the service class `\Siso\Bundle\PaymentBundle\Api\StandardPaymentService` and adapt the constant `EXCEEDED_ENTITY_AMOUNT_VALUE`. This constant is read via late static binding and will use the overridden value.

Example:

``` php
<?php

namespace Project\Bundle\ProjectBundle\Service;

use Siso\Bundle\PaymentBundle\Api\StandardPaymentService;

/**
 * Service class overide
 * <parameters>
 *     <parameter key="siso_payment.payment_service.class">Project\Bundle\ProjectBundle\Service\ExamplePaymentService</parameter>
 * </parameters>
 */
class ExamplePaymentService extends StandardPaymentService
{
    const EXCEEDED_ENTITY_AMOUNT_VALUE = 999999999.99;
}
```

## The redirection from the Shop to the Paygate fails because of an execution timeout

It could happen that during the encryption of the transaction data, the PHP process runs into an execution timeout. This would occur after clicking the "Buy now" button at the order summary page. The exception in the AJAX Response would look like:

```
Exception message:

Error: Maximum execution time of 60 seconds exceeded

Occured in file /var/www/project/vendor/jms/payment-core-bundle/JMS/Payment/CoreBundle/Cryptography/MCryptEncryptionService.php line 89
```

The problem is the usage of the PHP function [`mcrypt_create_iv`](http://php.net/manual/en/function.mcrypt-create-iv.php) in that line. The default implementation uses the /dev/urandom device in order to determine a random number. This could take a long time if the hosting system doesn't generate enough random events per time. And thus the call runs into a timeout.

### Solution(s)

Check the current available entropy on the system:

``` 
cat /proc/sys/kernel/random/entropy_avail
```

the result should be a number greater than 300, if less the entropy is not enough to generate random numbers.

To increase the entropy an application has to be installed.

#### haveged installation

Install the package with apt-get:

``` 
sudo apt-get install haveged
```

This tool will start automatically at boot and will keep entropy greater than 1000.
