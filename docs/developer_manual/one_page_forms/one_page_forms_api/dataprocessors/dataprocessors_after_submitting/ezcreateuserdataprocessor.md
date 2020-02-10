# EzCreateUserDataProcessor

The goal of this DataProcessor is to create a new EzUser. This data processor relies on the result of the CreateCustomerProfileDataDataProcessor, because the [CustomerProfileData](Customer-profile-data-model_23560898.html) object must be given.

The dataprocessor sets the following fields in eZ user:

  - login 
  - email
  - password
  - name
  - customer\_number
  - contact\_number
  - customer\_profile\_data

eZ Commerce provides a standard event for this handler which add a prefix to the login name of the user. The prefix can be defined in the config (data\_processor\_ez\_user\_login\_prefix). 

This event is defined in the service.yml:

``` 
<service id="ses_data_processor.pre_execute.create_ez_user_handler" class="%ses_data_processor.pre_execute.create_ez_user_handler.class%">
            <argument type="service" id="ezpublish.config.resolver" />
            <tag name="kernel.event_listener" event="ses_pre_execute_ses_forms.create_ez_user" method="preExecute" />
</service>
```

ID of this service:

ses\_forms.create\_ez\_user
