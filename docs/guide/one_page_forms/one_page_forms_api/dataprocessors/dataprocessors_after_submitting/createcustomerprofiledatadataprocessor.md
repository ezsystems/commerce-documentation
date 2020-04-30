# CreateCustomerProfileDataDataProcessor

The goal of this DataProcessor is to create new [CustomerProfileData](../../../../customers/customers_api/customer_profile_data_components/customer_profile_data_model.md) object and fill it with data from registration process.

ID of this service:

`ses.customer_profile_data.data_processor.create_customer_profile_data`

### Customer type

The customer type can be set in the configuration:

`forms.yml`:

``` yaml
parameters:
    ses_forms_values:
        ...
        customerType:
            private: private
            business: business
```
