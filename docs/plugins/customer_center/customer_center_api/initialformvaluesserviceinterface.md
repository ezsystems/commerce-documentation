# InitialFormValuesServiceInterface

## Introduction

This interface needs to be implemented by every service that is defined in the *initialFormValuesService* key of a form definition. For example in vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/config/customercenter.yml:

``` yaml
siso_customer_center.default.form.add_user:
    ...
    initialFormValuesService: siso_customer_center.initial_add_user_values_service
        ...
```

## InitialFormValuesServiceInterface

### public function getAttributesValues(array $formAttributes, CustomerCenterContact $customerContact = null);

This function will load the values for the form attributes from an external source, e.g. eZ, ERP or
CustomerProfileData and return the initial values for the form attributes.
The goal of this function is to pre-fill the form with predefined values.

The given $formAttributes define the attribute names that are suppose to be fetched and returned with values:
 - the static attributes may be fetched from eZ, ERP or CustomerProfileData
 - the dynamic attributes will be fetched only from eZ|
