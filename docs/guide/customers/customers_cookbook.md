# Customers cookbook

## How to manage / administrate delivery addresses

!!! tip

    All code examples in this document are written in the context of a ContainerAwareCommand implementation. Please note the code comments, as they give further implementation details.

!!! note

    Web-Connector \>= 3.0 for NAV with Web-Services is assumed to be the target system in these examples. For other ERP Systems and former versions of the Web-Connector / NAV, the process and data are likely to differ slightly. The external field 'Key', and the logic bound to it, is specific for this setup.

## Create a new delivery address in NAV

Delivery address management / -administration follows the standard process for all ERP messages. The following example depicts a simple implementation with hard coded address values. Please note the hint for the Code field. The Key field is explained later in the update message.

``` php
// Get the necessary services
$messageInq = $this->getContainer()->get('silver_erp.message_inquiry_service');
$messageTrans = $this->getContainer()->get('silver_erp.message_transport');

// Inquire the message object and prepare the request data
$msg = $messageInq->inquireMessage(CreateDeliveryAddressFactoryListener::CREATEDELIVERYADDRESS);
/** @var CreateDeliveryAddressRequest $request */
$request = $msg->getRequestDocument();
$request->DeliveryParty->PartyIdentification[0] = new DeliveryPartyPartyIdentification();
$request->DeliveryParty->PartyIdentification[0]->ID->value = '10000';
/* Hint: The address code generation is not managed in NAV. Unlike this hardcoded value,
 * the application must track existing address codes and ensure that no duplicated codes
 * are sent to the ERP.
 */
$request->DeliveryParty->SesExtension->value['Code'] = 'TEST';
$request->DeliveryParty->SesExtension->value['Blocked'] = 'false';
$request->DeliveryParty->PartyName = array(
    new DeliveryPartyPartyName(),
    new DeliveryPartyPartyName(),
);
// First name
$request->DeliveryParty->PartyName[0]->Name->value = 'Timothy';
// Last name
$request->DeliveryParty->PartyName[1]->Name->value = 'Tester';
$request->DeliveryParty->PostalAddress->StreetName->value = 'Testallee 1';
$request->DeliveryParty->PostalAddress->AdditionalStreetName->value = 'Gassenstr.';
$request->DeliveryParty->PostalAddress->CityName->value = 'Testow';
// Note: In NAV, only properly configured values are allowed for country codes.
$request->DeliveryParty->PostalAddress->CountrySubentityCode->value = '';
$request->DeliveryParty->PostalAddress->PostalZone->value = '12345';

// Send the data and process the response
/** @var CreateDeliveryAddressResponse $response */
$response = $messageTrans->sendMessage($msg)->getResponseDocument();
// The record key should be stored in the application's database for later updates.
$key = $response->DeliveryParty->SesExtension->value['Key'];
```

## Read an existing delivery address from NAV

In NAV, the address code is needed additionally to the party identification in order to fetch a delivery address.

``` php
// Get the necessary services
$messageInq = $this->getContainer()->get('silver_erp.message_inquiry_service');
$messageTrans = $this->getContainer()->get('silver_erp.message_transport');

// Inquire the message object and prepare the request data
$msg = $messageInq->inquireMessage(ReadDeliveryAddressFactoryListener::READDELIVERYADDRESS);
/** @var ReadDeliveryAddressRequest $request */
$request = $msg->getRequestDocument();
$request->DeliveryParty->PartyIdentification[0] = new DeliveryPartyPartyIdentification();
$request->DeliveryParty->PartyIdentification[0]->ID->value = '10000';
$request->DeliveryParty->SesExtension->value['Code'] = 'TEST';

// Send the data and process the response
/** @var ReadDeliveryAddressResponse $response */
$response = $messageTrans->sendMessage($msg)->getResponseDocument();
```

## Update an existing delivery address in NAV

In order to update an address, the complete data must be sent (changed and unchanged fields), together with the Key value of the last read data. The Key is a consistency check for the update. If the data in NAV changed since it was fetched the last time, the Keys of the update request and in NAV would mismatch and the request fails. In that case, the data must be fetched again and the current changes must be merged into the new data from NAV. This might need user interaction (i.e. several HTTP requests) and goes beyond the scope of this tutorial. After that, the merged data must be sent again in an update request with the new Key.

``` php
// Get the necessary services
$messageInq = $this->getContainer()->get('silver_erp.message_inquiry_service');
$messageTrans = $this->getContainer()->get('silver_erp.message_transport');

// Get the stored address data from the application's database
$deliveryParty = $this->fetchDeliveryAddress('10000', 'TEST');
    
// Inquire the message object and prepare the request data
$msg = $messageInq->inquireMessage(UpdateDeliveryAddressFactoryListener::UPDATEDELIVERYADDRESS);
/** @var UpdateDeliveryAddressRequest $request */
$request = $msg->getRequestDocument();
$request->DeliveryParty = $deliveryParty;
// Just an arbitrary extension of an existing value
$request->DeliveryParty->PostalAddress->StreetName->value .= '+';

// Send the data and process the response
/** @var UpdateDeliveryAddressResponse $response */
$response = $messageTrans->sendMessage($msg)->getResponseDocument();
if ($msg->getResponseStatus() === AbstractMessage::RESPONSE_STATUS_ERP_ERROR) {
    /* Evaluate if the error was because of a mismatch of the sent 'Key' value
     * If so:
     * - Fetch the current address data according to ReadDeliveryAddress
     * - Resolve possible conflicts with the changed data
     * - Retry the update with the new Key
     */
} else {
    // Standard error handling
}

// Store the newly retrieved key in the application's database for future updates
$lastKey = isset($response->DeliveryParty->SesExtension->value['Key'])
    ? $response->DeliveryParty->SesExtension->value['Key']
    : '';
```

## Delete an existing delivery address in NAV

For the deletion of addresses, only the Key is needed for the request. But again, the value must match in NAV, else the request is rejected and the data must be re-fetched in order to get the new Key (and new data for a potential review of the changes).

``` php
// Get the necessary services
$messageInq = $this->getContainer()->get('silver_erp.message_inquiry_service');
$messageTrans = $this->getContainer()->get('silver_erp.message_transport');

// Fetch the last stored Key
$lastKey = $this->getLastKey('10000', 'TEST');

// Inquire the message object and prepare the request data
$msg = $messageInq->inquireMessage(DeleteDeliveryAddressFactoryListener::DELETEDELIVERYADDRESS);
/** @var DeleteDeliveryAddressRequest $request */
$request = $msg->getRequestDocument();
$request->DeliveryParty->SesExtension->value['Key'] = $lastKey;

// Send the data and process the response
/** @var DeleteDeliveryAddressResponse $response */
$response = $messageTrans->sendMessage($msg)->getResponseDocument();
$status = $response->Result->value;
```
