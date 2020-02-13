# PreFillMyAccountDataProcessor

The goal of this data processor is to pre-fill the MyAccount form with data that are fetched from CustomerProfileData with the help of the [EzErpCustomerProfileDataService](Customer-profile-data-services_23560906.html).

``` 
ID of this service:
ses.customer_profile_data.data_processor.pre_fill_my_account
```

Used configuration

This configuration is used to define if first and surname from the form are stored in one or two seperate fields:

forms.yml

ses_forms.default.use_single_name_field: true/false
