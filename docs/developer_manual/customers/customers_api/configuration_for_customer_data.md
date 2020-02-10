# Configuration for customer data

### Different user groups for private and business customer

To separate business and private user in one installation two user groups in the user group *Shop user* are used. Next to the user groups a configuration is needed:

``` 
siso_core.default.user_group_location.business: 385
siso_core.default.user_group_location.private: 388
```

There is a default configuration in app/config/ecommerce\_parameters.yml, which uses the same loaction id for both user groups. This works if there is only one kind of users in a shop.

**Advanced version only**

For the advanced version the config is prepared in app/config/ecommerce\_advanced\_parameters.yml

### Timeout for ERP updates

The shop will check how old the customer data stored in the session is. If it is too old (timeout) the shop will fetch the data from the ERP again.

The timeout can be defined it a configuration file:

**EshopBundle/Resources/config/default\_values.yml**

``` 
 
 # the timeout in seconds, how long the remote data is valid (default 600s = 10m)
 silver_customer.config.default_values.remote_validation_timeout: 600
```

Currently the identifier of the attribute in eZ Platform (ContentType eZUser) has to be named 'customer\_number'.

### Default handling for VAT

The shop checks the default VAT handling using a configuration. If set to true the customer will get always prices including VAT. For BtoB shops the handling can be changed. The value can be overridden per customer if required (e.g. if the ERP provide this information per customer). Also some customer does not have to pay vat at all (e.g. abroad)

**EshopBundle/Resources/config/silver.eshop.yml**

``` 
#regulates if user see prices with or without vat in the shop
ses.customer_profile_data.isPriceInclVat: true
#regulates the vat handling - if set to false, vat is set to 0.0
ses.customer_profile_data.setHasToPayVat: true
```

## Working with contact data

### Enable/Disable fetching of the customer contact data:

**EshopBundle/Resources/config/default\_values.yml**

``` 
 # it is possible to enable either onPreEvent or onPostEvent or disable both

 # - onPreEvent: select contact is requested before requesting customer data
 # - onPostEvent: select contact is requested after requesting customer data
 # - disabled: disable select contact before/after requesting customer data 

 silver_customer.config.default_values.select_contact_mode: "onPostEvent"
```

##### **Set up the contact number**

The contact number should be set up in the Backend - In the Ez User Class as a field 'contact\_number'

**Contact number:** KT100210

### Connecting to the events

To take advantage from the existing events to f.e. modify some customer.contact data, your EventService must listen to 'ses\_ez\_erp\_customer\_pre\_remote\_data' and 'ses\_ez\_erp\_customer\_post\_remote\_data' Events:

``` 
 <service id="silver_customer.customer_event_service" class="%silver_customer.customer_event_service.class%">
            <tag name="kernel.event_listener" event="ses_ez_erp_customer_pre_remote_data" method="onPreRemoteData" />
            <tag name="kernel.event_listener" event="ses_ez_erp_customer_post_remote_data" method="onPostRemoteData" />           
 </service>
```

You also have to implement appropriate methods:

``` 
/**
     * Gives the opportunity to modify the customer via $customerEvent->getCustomer()
     * before retrieving the customer data form ERP
     * @param AbstractCustomerEvent $customerEvent
     */
    public function onPreRemoteData(AbstractCustomerEvent $customerEvent)
    {
        if ($this->selectContactOptions['onPreEvent']) {
            $this->setContactData($customerEvent);
        }
    }
    /**
     * Gives the opportunity to modify the customer via $customerEvent->getCustomer()
     * after retrieving the customer data form ERP
     * @param AbstractCustomerEvent $customerEvent
     */
    public function onPostRemoteData(AbstractCustomerEvent $customerEvent)
    {
        if ($this->selectContactOptions['onPostEvent']) {
            $this->setContactData($customerEvent);
        }
    }
```

## User edit interface configuration for backend

It's possible to edit additional form fields for buyer address and delivery addresses in the backend extended for the standard address. The additional fields are stored in ses extension of the address. If the configured ses\_extension field does not exist it will be created.

**Default configuration**

``` 
siso_core.default.additional_buyer_ses_extension_form_fields: []
siso_core.default.additional_delivery_ses_extension_form_fields: []
```
