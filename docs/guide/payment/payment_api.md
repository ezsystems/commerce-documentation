# Payment API

## Silversolutions Payment Bundle

Additional payment providers can be implemented as described below. See [Building new payment plugins](payment_cookbook/building_new_payment_plugins.md) as well.

The `SilversolutionsPaymentBundle` defines and implements an interface to the `JMSPaymentCoreBundle`. The main interface is represented by the `PaymentServiceInterface`, which is the public access point to payment processing.

## PaymentServiceInterface

The method that starts all payment processes is `processPayment()` of the `PaymentServiceInterface`. It tries to completely process the payment and would throw a `RedirectionRequiredException` if the payment method represented by the `$paymentMethodIdentifier` needs an HTTP redirection to a payment portal site in order to progress in the payment (for example for an input of critical data like a credit-card number). The calling instance must take responsibility for this exception and handle the redirection accordingly.

There is several data necessary in order to start a payment process:

- The `$orderId` is a valid identifier for the instance, which holds the order data
- The `$paymentMethodIdentifier` is a string, which uniquely identifies a payment plugin implementation (e.g. for PayPal Express Checkout)
- The `$amount` parameter holds the amount to be paid (in the lowest available unit of a currency, e.g. Euro cents)
- `$currency` holds a string, which identifies the currency of the $amount
- The array `$extendedData` can be used for specific configuration of the given `$paymentMethodIdentifier` and thus the respective payment plugin (This parameter is optional)

The payment process can be concluded by the method `finalizePayment()` if it needed some user interaction (i.e. authorize the transaction at a payment gateway). This method may also throw the `RedirectionRequiredException`, in the case the respective payment needs further user interactions. The `$orderId` parameter must pass the identifier for the order instance, that was also passed to `processPayment()`. The optional `$additionalData` parameter can be used to pass further information to the payments external data. For example: extract important fields from the payment gateways response.

After the payment process was successfully processed, it is possible to get the respective transaction ID, that was provided by the payment gateway. For this, the interface provides the method `determinePaymentTransactionId()`.

### The interface signature

``` php
interface PaymentServiceInterface
{
    /**
     * @param string $orderId
     * @param string $paymentMethodIdentifier
     * @param int $amount
     * @param string $currency
     * @param array $extendedData
     * @throws RedirectionRequiredException
     */
    public function processPayment(
        $orderId,
        $paymentMethodIdentifier,
        $amount,
        $currency,
        array $extendedData = array()
    );

    /**
     * @param string $orderId
     * @param array $additionalData
     * @throws RedirectionRequiredException
     */
    public function finalizePayment($orderId, $additionalData = array());

    /**
     * @param string $orderId
     * @return string
     */
    public function determinePaymentTransactionId($orderId);
}
```

### Simple example of starting the payment process

``` php
/** @var PaymentServiceInterface $paymentService */
$paymentService = $this->get('siso_payment.payment_service');
$copiedBasket = $basketService->copyBasket($basket);
try {
    $copiedBasket->setState(BasketService::STATE_OFFERED);
    $basketService->storeBasket($copiedBasket);
    $paymentService->processPayment(
        $copiedBasket->getBasketId(),
        $copiedBasket->getPaymentMethod(),
        $copiedBasket->getTotalsSum()->getTotalNet(),
        $copiedBasket->getTotalsSum()->getCurrency()
    );
    // Store the transaction ID where it is available for the ERP communication
    $this->storeTransactionId(
        $paymentService->determinePaymentTransactionId($orderId)
    );
    $copiedBasket->setState(BasketService::STATE_PAYED);
} catch (RedirectionRequiredException $ex) {
    $redirectUrl = $ex->getUrl();
    // Invoke an HTTP redirect to the payment portal on the Users HTTP-client 
    $this->performHttpRedirect($redirectUrl);
}
```

### Simple example of finalizing the payment process

``` php
$additionalData = array(
    'notify_data' => $request->request->all(),
);
$paymentService->finalizePayment($orderReference, $additionalData);
$transactionId = $paymentService->determinePaymentTransactionId($orderReference);
/** @var Basket $order */
$order = $this->fetchOrderEntity($orderReference);
$order->setPaymentTransactionId($transactionId);
$order->setState(BasketService::STATE_PAYED);
$this->storeOrder($order);
$orderSucessfull = $this->submitOrderToErp($order);
if ($orderSucessfull) {
    $order->setState(BasketService::STATE_CONFIRMED);
    $this->storeOrder($order);
}
```

## PluginConfigurationInterface

For the payment plugin of `telecash_connect` (and the most of payment gateways) it is necessary to store the return URLs in the extended data of the PaymentInstruction entity. This is because the related basket/order ID needs to be appended somehow to the URL in order to determine the basket and the related payment instruction for the further processing in the returning URL requests. Thus, it is not possible to simply configure the URLs via the symfony container, as the basket ID is determined dynamically.

In order to provide an adaptable way to integrate the various data fields, which might be used by the different payment plugins for storing e.g. these URL information, this interface is used. For every payment plugin, that should be available in the eZ Commerce, it is necessary to implement this interface (if more than the standard fields, like amount, are necessary for the payment process).

The interface defines a single method, `createExtendedDataForOrder($orderReference)`, which must build and return the respective configuration data within an associative PHP array.

### The interface signature

``` php
interface PluginConfigurationInterface
{
    /**
     * Must return an associative array, which contains specific data for
     * the payment plugin, the implementation is intended for.
     *
     * e.g.:
     * array(
     *     'return_url' => 'http://example.com/order-confirmation/id1234',
     *     'error_url' => 'http://example.com/order-failed/id1234',
     * )
     * PLEASE MIND that the field names in this example are arbitrary.
     *
     * @param string $orderReference
     * @return array
     */
    public function createExtendedDataForOrder($orderReference);
}
```

### Service definition

In order to register an implementation of this interface to the `StandardPaymentService` you will need to define it as a service and tag it. The **tag** name is `payment.plugin.config` and it needs an attribute named `paymentMethod`, which must be the same as the plugins form **alias**.

Example:

``` xml
<service id="payment.plugin.telecash_connect.config"
         class="%payment.plugin.telecash_connect.config.class%">
    <tag name="payment.plugin.config" paymentMethod="telecash_connect" />
</service>
```

## StandardPaymentService

The `StandardPaymentService` implements the `PaymentServiceInterface`. In order to store the relation between the shop's basket entities and the payment entities, a new relation entity was defined: `PaymentInstructionBasketMap`. The service also uses the `PluginConfigurationInterface`, which is used to inject the respective payment configuration into the `PaymentInstruction`.

The service's standard definition is the following:

``` xml
<parameters>
    <parameter key="siso_payment.payment_service.class">Siso\Bundle\PaymentBundle\Api\StandardPaymentService</parameter>
</parameters>
<services>
    <service id="siso_payment.payment_service"
             class="%siso_payment.payment_service.class%">
        <argument type="service" id="doctrine.orm.entity_manager" />
        <argument type="service" id="payment.plugin_controller" />
    </service>
</services>
```
