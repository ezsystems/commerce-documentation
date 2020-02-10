# CreateCustomerProfileDataDataProcessor

The goal of this DataProcessor is to create new [CustomerProfileData](Customer-profile-data-model_23560898.html) object and fill it with data from registration process.

ID of this service:

ses.customer\_profile\_data.data\_processor.create\_customer\_profile\_data

### Customer type

The customer type can be set in the configuration:

**forms.yml**

``` 
parameters:
    ses_forms_values:
        ...
        customerType:
            private: private
            business: business
```
