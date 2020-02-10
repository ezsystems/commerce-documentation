#  Building new payment plugins 

This document describes which steps are necessary to implement a new payment plugin. Since the SisoPaymentBundle is based on the JMSPaymentCoreBundle, the steps overlap with the requirements for [new payment plugins for JMS payment](http://jmsyst.com/bundles/JMSPaymentCoreBundle/master/plugins).

### Steps to be done:

1.  Create a new plugin class  
    `Bundle\PaymentTestBundle\Plugin\TelecashConnectPlugin`
      - [Create a payment method identifier](#Buildingnewpaymentplugins-new_plugin_class)
      - [Override transaction methods](#Buildingnewpaymentplugins-override_transaction_method)
2.  Create a [new form type class](#Buildingnewpaymentplugins-new_form_type_class)  
    `Bundle\PaymentTestBundle\Form\Type\TelecashConnectType`
3.  Create a [new configuration class](#Buildingnewpaymentplugins-new_configuration_class) and define plugin specific data  
    `Bundle\PaymentTestBundle\Plugin\`Configuration
4.  [Register services](#Buildingnewpaymentplugins-register_services)

## Create new plugin class

Create a new plugin class, which extends* *`\JMS\Payment\CoreBundle\Plugin\AbstractPlugin`

<span id="Buildingnewpaymentplugins-new_plugin_class" class="confluence-anchor-link">

### Create a payment method identifier

You'll need to implement the abstract method *processes($paymentSystemName)*. *$paymentSystemName* is the identifier for the payment methods this plugin will support. The identifier will be used e.g. in the payment form in the checkout process.

``` 
<?php

namespace Bundle\PaymentTestBundle\Plugin;

use JMS\Payment\CoreBundle\Plugin\AbstractPlugin;

/**
 * JMS Plugin class for the payment
 */
class TeleCashConnectPlugin extends AbstractPlugin
{
    /**
     * Implementation which determines, that this plugin will be able to handle
     * the 'telecash_connect' payment system.
     *
     * @param string $paymentSystemName
     * @return boolean
     */
    public function processes($paymentSystemName)
    {
        return $paymentSystemName === 'telecash_connect';
    }
}
```

Important\!

The payment method identifier MUST comply the tag-attribute **alias** in the *form type definition* and the tag-attribute **paymentMethod** in the *extended data configuration* definition ([described below](#Buildingnewpaymentplugins-register_services)).

<span id="Buildingnewpaymentplugins-override_transaction_method" class="confluence-anchor-link">

### Override the supported transaction methods

The `\JMS\Payment\CoreBundle\Plugin\PluginInterface` defines several methods for specific transactions. `\JMS\Payment\CoreBundle\Plugin\AbstractPlugin` implements the `PluginInterface`, but only throws the *FunctionNotSupportedException*. Thus, deriving classes must override specific methods and implement the transaction logic. You'll need to implement **at least one** of these methods.

  - For the payment operation AUTHORIZE, you will need to implement the interface method: ***`approve()`***
  - In our example, we will implement ***approveAndDeposit()***. This method is used for the SALE payment operation, which we will cover later on.

#### *approveAndDeposit*()

In this example, a financial transaction is expected to have one of two states:

  - STATE\_NEW 
  - STATE\_PENDING

Possible misbehavior:

1.  Any other state is ignored and would cause a rollback
2.  Any other thrown Exception than *ActionRequiredException* would cause a rollback

In case of rollback the *JMS PluginController* will set the state to STATE\_FAILED and rolling back the transaction as the controller expects:

  - either the response code of the transaction will be set to RESPONSE\_CODE\_SUCCESS

  - or the method throws an *ActionRequiredException*.

``` 
    /**
     * @param FinancialTransactionInterface $transaction
     * @param bool $retry
     * @throws ActionRequiredException
     * @throws \Exception
     */
    public function approveAndDeposit(FinancialTransactionInterface $transaction, $retry)
    {
        // Get transaction data
        $extendedData = $transaction->getExtendedData();
        // Check the status of the transaction
        switch ($transaction->getState()) {
            case FinancialTransactionInterface::STATE_NEW;
                // Check $data, if redirect is necessary
                // collect data for necessary redirect action
                // prepare ActionRequiredException with data and throw it
                $instruction = $transaction->getPayment()->getPaymentInstruction();
                $amount = $transaction->getRequestedAmount();
                $currency = $this->parameters['currency_mapping'][$instruction->getCurrency()][1];
                $extendedData->set('transaction_type', self::TRX_TYPE_APPROVE_AND_DEPOSIT);
                $formData = $this->buildFormData($amount, $currency, $extendedData);
                $extendedData->set('form_data', $formData);
                $transaction->setResponseCode(PluginInterface::REASON_CODE_ACTION_REQUIRED);
                $exception = new ActionRequiredException();
                $exception->setAction(new VisitUrl('/telecash/forward'));
                throw $exception;
                break;
            case FinancialTransactionInterface::STATE_PENDING;
                // Verify notify data and finalize payment if the data is valid
                $notifyData = $extendedData->get('notify_data');
                $notifyValidationHash = $this->createNotifyHash(
                    $notifyData['chargetotal'],
                    $notifyData['currency'],
                    $extendedData->get('storename'),
                    $extendedData->get('secret'),
                    $notifyData['txndatetime'],
                    $notifyData['approval_code']
                );
                if ($notifyValidationHash !== $notifyData['notification_hash']) {
                    throw new \Exception('Notify hash not valid! This HTTP request is probably a fraud.');
                }
                // if approval_code starts with an Y, the transaction is finalized,
                // else the notify wasn't final.
                if (substr($notifyData['approval_code'], 0, 1) === self::TRX_REPONSE_CODE_OK) {
                    $processedAmount = $notifyData['chargetotal'];
                    $transactionId = $notifyData['oid'];
                    $transaction->setProcessedAmount($processedAmount);
                    $transaction->setReferenceNumber($transactionId);
                    $transaction->setResponseCode(PluginInterface::RESPONSE_CODE_SUCCESS);
                }
                break;
        }
    }
```

<span id="Buildingnewpaymentplugins-new_form_type_class" class="confluence-anchor-link">

## Create new form type class

If you want to display the new implemented payment plugin as an option in the checkout process, you have to create a form type class and define it as a service in the DI container configuration.

``` 
<?php

namespace Bundle\PaymentTestBundle\Form\Type;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;

class TeleCashConnectType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
    }

    public function getBlockPrefix()
    {
        return 'telecash_connect';
    }
}
```

<span id="Buildingnewpaymentplugins-new_configuration_class" class="confluence-anchor-link">

## Create new configuration class

In the configuration class you can **define payment plugin specific extended data.**

Most payment provider implementations will need more than data that is defined by the *JMSPaymentCoreBundle*'s model. For this reason *extended data* is used.

This data can be prepared and provided by implementing the `\Siso\Bundle\PaymentBundle\Api\PluginConfigurationInterface`.

In this example two methods are implemented:

  - *createExtendedDataForOrder*()
      - provides the return URLs, which are sent to the payment gateway during the redirect request and some other necessary values for the plugin implementation.
  - *determinePaymentOperation*()
      - returns the constant PO\_SALE. This causes the *StandardPaymentService* to use [approveAndDeposit()](#Buildingnewpaymentplugins-override_transaction_method) in order to try to finalize the payment. This method can be used to invoke different payment operations depending on, for example, some configuration.

This PluginConfigurationInterface is designed to be injected as a service. Which means you can inject any needed dependency to this class via the service container, as well. In this case it is the symfony router and some parameter values.

``` 
namespace Bundle\PaymentTestBundle\Plugin;

use Siso\Bundle\PaymentBundle\Api\PluginConfigurationInterface;
use Symfony\Component\Routing\RouterInterface;

/**
 * Configuration class for the Telecash payment plugin.
 */
class Configuration implements PluginConfigurationInterface
{
    /**
     * @var RouterInterface
     */
    private $router;

    /**
     * @var array
     */
    private $parameters;

    /**
     * @param RouterInterface $router
     * @param array $parameters
     */
    public function __construct(RouterInterface $router, array $parameters = array())
    {
        $this->router = $router;
        $this->parameters = $parameters + array(
            'timezone' => '',
            'storename' => '',
            'secret' => '',
            'mode' => '',
        );
    }

    /**
     * @param string $orderReference
     * @return array
     */
    public function createExtendedDataForOrder($orderReference)
    {
        return array(
            'success_url' => $this->router->generate(
                'siso_telecash_success',
                array(
                    'orderReference' => $orderReference,
                ),
                RouterInterface::ABSOLUTE_URL
            ),
            'error_url' => $this->router->generate(
                'siso_telecash_error',
                array(
                    'orderReference' => $orderReference,
                ),
                RouterInterface::ABSOLUTE_URL
            ),
            'notify_url' => $this->router->generate(
                'siso_telecash_notify',
                array(
                    'orderReference' => $orderReference,
                ),
                RouterInterface::ABSOLUTE_URL
            ),
            'timezone' => $this->parameters['timezone'],
            'storename' => $this->parameters['storename'],
            'secret' => $this->parameters['secret'],
            'mode' => $this->parameters['mode'],
        );
    }

    /**
     * Returns PluginConfigurationInterface::PO_SALE as no other payment
     * operation is currently supported.
     *
     * @return string
     */
    public function determinePaymentOperation()
    {
        // Only sale (approveAndDeposit) is currently supported by this plugin.
        return PluginConfigurationInterface::PO_SALE;
    }
} 
```

<span id="Buildingnewpaymentplugins-register_services" class="confluence-anchor-link">

## Register necessary services

In order to get the new PaymentPlugin recognized by the JMSPaymentBundle, the new created classes must be registered as services with specific **tags**.

``` 
    <parameters>
        <parameter key="payment.form.telecash_connect_type.class">Bundle\PaymentTestBundle\Form\Type\TelecashConnectType</parameter>
        <parameter key="payment.plugin.telecash_connect.class">Bundle\PaymentTestBundle\Plugin\TelecashConnectPlugin</parameter>
        <parameter key="payment.plugin.telecash_connect.config.class">Bundle\PaymentTestBundle\Plugin\Configuration</parameter>
    </parameters>

    <services>
        <service id="payment.plugin.telecash_connect.config"
                 class="%payment.plugin.telecash_connect.config.class%">
            <argument type="service" id="router" />
            <argument type="collection">
                <argument key="mode" type="string">%siso_telecash_payment.parameter.mode%</argument>
                <argument key="storename" type="string">%siso_telecash_payment.parameter.storename%</argument>
                <argument key="secret" type="string">%siso_telecash_payment.parameter.secret%</argument>
                <argument key="timezone" type="string">%siso_telecash_payment.parameter.timezone%</argument>
            </argument>
            <tag name="payment.plugin.config" paymentMethod="telecash_connect" />
        </service>

        <service id="payment.plugin.telecash_connect"
                 class="%payment.plugin.telecash_connect.class%">
            <call method="setParameters">
                <argument type="collection">
                    <argument key="currency_mapping">%siso_payment.currency_mapping%</argument>
                    <argument key="test_code">%siso_payment.telecash.test_code.success%</argument>
                    <argument key="application_mode">%siso_telecash_payment.application_mode%</argument>
                </argument>
            </call>
            <tag name="payment.plugin" />
        </service>

        <service id="payment.form.telecash_connect_type"
                 class="%payment.form.telecash_connect_type.class%">
            <tag name="payment.method_form_type" />
            <tag name="form.type" alias="telecash_connect" />
        </service>
    </services>
```

### Services & Tags

#### Payment plugin

    Bundle\PaymentTestBundle\Plugin\TelecashConnectPlugin

Every payment plugin service is injected into the JMS PluginManager by the **tag** *payment.plugin*.

#### Form values

    Bundle\PaymentTestBundle\Form\Type\TelecashConnectType

[Compiler pass](Payment---FAQ_23560268.html) will search for the **tag** *form.type* and uses the **alias**. The compiler pass itself is defined in the `SisoCheckoutBundle`, as the form is part of the checkout process.

The **alias** is used:

  - as the [payment method identifier](#Buildingnewpaymentplugins-new_plugin_class)
  - in the front-end - as the option (value) in the payment form in the checkout process

#### Extended data configuration

    Bundle\PaymentTestBundle\Plugin\Configuration

The configuration service is defined by the **tag** *payment.plugin.config*, which MUST provide the attribute **paymentMethod** with the value according to the respective [payment method identifer](#Buildingnewpaymentplugins-new_plugin_class).
