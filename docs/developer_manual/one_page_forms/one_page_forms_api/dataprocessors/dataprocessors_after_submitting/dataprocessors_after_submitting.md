# dataProcessors (after submitting) 

Any number of data processors can be executed after the form was submitted. The configuration takes an array of data processors that will be executed one after another.

**Configuration example**

``` 
 ses_forms.configs.business_activation:
        ...
        dataProcessors:
            - ses_forms.validate_business_activation
            - ses.customer_profile_data.data_processor.create_customer_profile_data
            - ses_forms.create_ez_user
            - ses_forms.login_new_ez_user 
```

# FormDataProcessorException

If one of the data processors throws an FormDataProcessorException this will stop further processing.

The user will see the filled form with the error message from FormDataProcessorException.
