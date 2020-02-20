# Order Submission

Depending on the chosen payment method, the order is completed after:

- submitting the [order summary](../../../../../../developer_manual/checkout/checkout_api/forms/checkout_summary_form/order_summary.md)
- receiving a notification from the payment gateway
- returning from the payment gateway

During completion, the order data is sent to the ERP system. On success, the state of the submitted basket changes to `confirmed`. If it fails, the state becomes `ordered_failed` and email is sent to administrator.

## BasketGuidService

After the summary of the checkout was confirmed by the user, the payment process is initiated. In order to identify the order across all systems (shop, payment gateway, ERP system), a GUID is generated and stored to the basket. The generation of that ID is left to a dedicated service. This way it is easy to override the standard generation routine.

- Service ID: `silver_basket.guid_service`
- Standard service class: `\Silversolutions\Bundle\EshopBundle\Services\BasketGuidService`

## WebConnectorErpService

The AbstractErpService defines a method:

`submitOrder(Basket $basket) : OrderResponse`

The standard implementation of this method is found in the

`\Silversolutions\Bundle\EshopBundle\Services\WebConnectorErpService`

This method prepares the [CreateSalesOrderMessage](../../../erp_communication/erp_components\erp_component_messages/erp_message_calculatesalesorder_createsalesorder.md) out of the basket's data and sends it to the ERP using the [ERP Transport](../../../erp_communication/erp_components/erp_component_transport.md). In case of an error during the remote communication the [failed order process](../failed_order_process/failed_order_process.md) is triggered via the OrderFailedEvent.

## Event before order is submitted

Before the order is sent to the ERP an event is triggered. The corresponding event listener can modify the basket data or perform any additional action. It can set the status to 'failed' and set en error message.

In a case, that the event status is 'failed', the order will not be submitted and will be treated as a lost order.

It is not possible to use services in appropriate event listeners, that requires **session**, because the process can be executed also from command line interface in the lost order process\!

For example it is not possible to use the CustomerProfileDataService ("ses.customer\_profile\_data.ez\_erp"), and eZRepository ("ezpublish.api.repository") must be used instead in order to fetch the user data.

### How to listen to this event?

There is already a dummy CreateCustomerListener. The goal of this listener is to create a customer in ERP before the order is submitted. If creating of the customer fails, the status 'failed' must be returned.

This listener is not fully implemented yet and performs only prove of concept for the pre\_submit\_order events.

event identifier

`event="siso_erp.pre_submit_order"`

Example: CreateCustomerListener:

``` xml
<service id="siso_core.create_customer_listener" class="%siso_core.create_customer_listener.class%">
    <argument type="service" id="silver_erp.facade" />    
    <argument type="service" id="ezpublish.api.repository" /> 
    <argument>$create_customer_listener_active;siso_core$</argument>    
    <tag name="kernel.event_listener" event="siso_erp.pre_submit_order" method="createCustomer" priority="10" />  
</service>
```

##### How to disable the CreateCustomerListener?

``` yaml
parameters:
    siso_core.default.create_customer_listener_active: false
```

## Sending additional data in the order

In order to extend the message with additional fields, there are two possible ways:

- Extend the CreateSalesOrderMessage class
    - This solution causes the risk of maintance effort in case of an update of the shop's base implementation, as changes to the base message must be merged to the extended version.
- write the data into [one of the SesExtension](../../../erp_communication/erp_components\erp_component_messages/erp_message_calculatesalesorder_createsalesorder.md) [tree elements (see ses_tree in table)](../../../erp_communication/erp_components\erp_component_messages/erp_message_class_generator.md)).
    - This solution should only be used to add simple, single valued fields. During the communication with the remote service (e.g. Web-Connector), the tree attributes are mapped between PHP arrays and XML data implicitly without any structure definition. This might cause conflicts for XML Elements, that occur multiple times, and sequential PHP arrays.

### Example

Sometimes it is necessary to send additional (project specific data) in the order. You can get this information maybe from basket (dataMap), or even from customer data and need to send them to ERP.

In that case, you should implement an event listener which subscribes to the **MessageRequestEvent**.

#### Service implementation

``` php
class MessageRequestListener
{
    /**
     * This method must be registered as a listener to the
     * 'silver_eshop.request_message' event.
     *
     * @param MessageRequestEvent $event
     */
    public function onOrderRequest(MessageRequestEvent $event)
    {
        $message = $event->getMessage();
        if ($message instanceof CreateSalesOrderMessage) {
            /** @var Order $request */
            $request = $message->getRequestDocument();
            $userId = $request->BuyerCustomerParty->SupplierAssignedAccountID->value;           
         
            if (!empty($userId)) {
                try {
                    $userService = $this->ezRepository->getUserService();
                    $ezUser = $userService->loadUser($userId);
                    if ($ezUser->getFieldValue(EzHelperService::USER_CUSTOMER_PROFILE_DATA)->text != '') {
                        $customerProfileDataBase64Serialize = $ezUser->getFieldValue(EzHelperService::USER_CUSTOMER_PROFILE_DATA)->text;
                        $customerProfileDataSerialized = base64_decode($customerProfileDataBase64Serialize);
                        $customerProfileData = unserialize($customerProfileDataSerialized);
                        //read customer data and set them in the SesExtension field
                        if ($customerProfileData instanceof CustomerProfileData) {
                            $customerType = $customerProfileData->sesUser->customerType;                           
                            $title = $customerProfileData->getDataMap()->hasAttribute('title')
                                ? $customerProfileData->getDataMap()->getAttribute('title') : '';
                            $academicTitle = $customerProfileData->getDataMap()->hasAttribute('academicTitle')
                                ? $customerProfileData->getDataMap()->getAttribute('academicTitle') : '';                          
                           
                            $request->SesExtension->value['customerType'] = isset($customerType) ? $customerType : '';                     
                            $request->SesExtension->value['title'] = isset($title) ? $title : '';
                            $request->SesExtension->value['academicTitle'] = isset($academicTitle) ? $academicTitle : ''; 
                    }
                } catch (\Exception $e) {
                    // must be injected, somehow
                    $this->logger->error('Exception:' . $e->getMessage());
                }
            }
        }
    }
}
```

#### Service configuration

``` 
   <parameter key="siso_checkout.message_request_listener.class">Path\To\Your\Class\MessageRequestListener</parameter>
...

   <service id="siso_checkout.message_request_listener" class="%siso_checkout.message_request_listener.class%">           
       <tag name="kernel.event_listener" event="silver_eshop.request_message" method="onOrderRequest" />
   </service>
```
