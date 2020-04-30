# Customers API

### How to get data in PHP

This example gives access to the data available for the current user. If the call is the first call after a login the data from the ERP ist fetched automatically in case the user has a customer number. 

``` php
$customer = $this->get('silver_customer.customer_service')->getCurrentCustomer();
 
// now, you can access any public method/member from the customer object, e.g.:
 
$customerNumber = $customer->getCustomerNumber();
$email = $customer->getEmail();
 
/** @var $deliveryAddresses \Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\Party */
$deliveryAddresses = $customer->getDeliveryAddresses();
 
```

If the ERP provides further information you will find this in an SesExtension Attribute:

``` php
$postingGroup = $deliveryAddresses[0]->SesExtension->value['CustomerPostingGroup'];
```

If you want to access data from the eZ user object. Please note that the ContentField in eZ is prefixed with a `ez_` (The field in eZ user ContentType has the identifier "testfield"):

``` php
$customerProfileDataService = $this->get('ses.customer_profile_data.ez_erp');
$customerProfileData = $customerProfileDataService->getCustomerProfileData();

$testfield = $customerProfileData->getDataMap()->getAttribute('ez_testfield');
```

### Request a customer by number from the ERP

``` php
$erpService = $this->getContainer()->get('silver_erp.facade');
$selectCustomerResponse = $erpService->selectCustomer($no);
```
