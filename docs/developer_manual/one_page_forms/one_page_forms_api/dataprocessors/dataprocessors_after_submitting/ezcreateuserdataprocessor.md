# EzCreateUserDataProcessor

The goal of this DataProcessor is to create a new EzUser. This data processor relies on the result of the CreateCustomerProfileDataDataProcessor, because the [CustomerProfileData](../../../../customers/customers_api/customer_profile_data_components/customer_profile_data_model.md) object must be given.

The dataprocessor sets the following fields in eZ user:

- login 
- email
- password
- name
- customer_number
- contact_number
- customer_profile_data
- ses_hastopay_vat
- ses_display_vat

eZ Commerce provides a standard event for this handler which add a prefix to the login name of the user. The prefix can be defined in the config (data_processor_ez_user_login_prefix). 

This event is defined in the service.yml:

``` xml
<service id="ses_data_processor.pre_execute.create_ez_user_handler" class="%ses_data_processor.pre_execute.create_ez_user_handler.class%">
            <argument type="service" id="ezpublish.config.resolver" />
            <tag name="kernel.event_listener" event="ses_pre_execute_ses_forms.create_ez_user" method="preExecute" />
</service>
```

ID of this service:

`ses_forms.create_ez_user`

Values set for private/business registration:

Private:

```
ses_hastopay_vat = true
ses_display_vat = true
```

Business:

```
ses_hastopay_vat = true
ses_display_vat = false
``
