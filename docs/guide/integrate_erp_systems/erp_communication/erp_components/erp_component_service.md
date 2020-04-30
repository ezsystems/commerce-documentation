# ERP Component: Service

To provide a simple interface to the ERP communication, an abstract class is defined. This class is intended to be derived from Symfony service classes. For example the WebConnectorErpService implements this abstract class for the communication to the silver.solutions Web-Connector.

For every basic request to the ERP one class methods exists (e.g. for price calculation of some products). Currently this includes:

``` php
/**
 * Sends a request to the ERP which queries the ERP specific price
 * calculation. The PriceRequest contains all information that is necessary
 * for the ERP's price calculation. For more information: @see PriceRequest.
 *
 * @param PriceRequest $priceRequest
 * @return OrderResponse|null
 */
public function calculatePrices(PriceRequest $priceRequest);

/**
 * Sends a request to the ERP which cause the creation of an order. The
 * request is built out of the given basket object. If the request was
 * successful, an OrderResponse object is returned, otherwise null.
 *
 * @param Basket $basket
 * @param array $params
 * @return OrderResponse|null
 */
public function submitOrder(Basket $basket, array $params = array());

/**
 * Requests information about the customer from the ERP. This includes data
 * like addresses etc.. If multiple records may fit to the requested
 * $customerNumber filter, the parameter $maxCount may limit result.
 *
 * $maxCount may be ignored, if the targeted ERP system does not support
 * multiple customer records, using the customer number.
 *
 * @param string $customerNumber
 * @param int $maxCount
 * @param array $params
 * @return CustomerResponse
 */
public function selectCustomer($customerNumber, $maxCount = 1, array $params = array());

/**
 * @param string $customerNumber
 * @param string $contactNumber
 * @param array $params
 * @return null|Contact
 */
public function selectContact($customerNumber, $contactNumber = '', array $params = array());
```

## Service method implementation

Within a method all necessary steps for the complete requests have to be done. The implementation for selectCustomer() could look like the following (This is not an actual implementation. It just demonstrates the workflow):

``` php
class ExampleErpService extends AbstractErpService
{
    /**
     * @var Silversolutions\Bundle\EshopBundle\Services\MessageInquiryService
     */
    protected $messageInquiry;
    /**
     * @var Silversolutions\Bundle\EshopBundle\Services\Transport\AbstractMessageTransport
     */
    protected $transport;

    // {...}
    
    public function selectCustomer($customerNumber, $maxCount = 1, array $params = array())
    {
```

At first you invoke the message factory to create an instance of the ERP message you want to send.

``` php
        // try to get message instance
        try {
            /** @var SelectCustomerMessage $selectCustomerMessage */
            $selectCustomerMessage = $this->messageInquiry->inquireMessage(
                StandardMessageFactoryListener::SELECTCUSTOMER
            );
        } catch (MessageInquiryException $messageException) {
            // {Do some logging or appropriate exception handling}
            return null;
        }
```

After that you have to feed the data, which were given to the method, into the received message object.

``` php
        // initialize request values and send message
        if (!$selectCustomerMessage instanceof SelectCustomerMessage) {
            $context = array('message' => $selectCustomerMessage);
            // {Do some logging or appropriate exception handling}
            return null;
        }
        $selectCustomerMessage->setCustomerNumber($customerNumber);
        $selectCustomerMessage->setMaxCount($maxCount);
```

Then you have to get an instance of the transport service and give the message instance to the sendMessage() method. It will return the same instance, which you passed as argument, but if everything went fine, that instance will hold the response now.

``` php
        try {
            $response = $this->transport->sendMessage($selectCustomerMessage)->getResponseDocument();
            if (!$response instanceof OrderResponse) {
                $context = array('response' => $response);
                // {Do some logging or appropriate exception handling}
                return null;
            }
        } catch (\RuntimeException $rtException) {
            // {Do some logging or appropriate exception handling}
            return null;
        }
```

Now you can return the response of the message directly or prepare the response, that is defined by this service method.

``` php
        return $response;
    }
```

## Symfony service configuration

If you reimplement the abstract service, you have to register your class to the symfony service container (I use YAML syntax here due to simpler presentation):

``` yaml
services:
    silver_erp.facade:
        class:     "\Example\Namespace\ExampleErpService"
        arguments: ["@silver_erp.message_inquiry_service", "@silver_erp.message_transport"]
```

The service ID has to be `silver_erp.facade`. That way this implementation is automatically used wherever the ERP service is injected as a dependency. The arguments in this example are another two services of the ERP bundle. These are injected as dependency of the example service class. Since the message inquiry service and the transport service are necessary for all ERP communication, you these two dependency will be the least of all ERP service implementations.

This redefines the default service configuration. Make sure that your configuration has a higher priority than the ERP bundle in your Symfony project setup.
