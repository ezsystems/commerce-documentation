# InitialFormValuesServiceInterface

`InitialFormValuesServiceInterface` must be implemented by every service that is defined in the `initialFormValuesService` key of a form definition.
For example, in `CustomerCenterBundle/Resources/config/customercenter.yml`:

``` yaml
siso_customer_center.default.form.add_user:
    ...
    initialFormValuesService: siso_customer_center.initial_add_user_values_service
        ...
```

## getAttributesValues

The `getAttributesValues` method loads the values for the form attributes from an external source,
e.g. content model, ERP or `CustomerProfileData`, and returns the initial values for the form attributes.
The goal of this function is to pre-fill the form with predefined values.

The given `$formAttributes` define the attribute names that are suppose to be fetched and returned with values:

 - the static attributes may be fetched from the content model, ERP or `CustomerProfileData`
 - the dynamic attributes are fetched only from content

## InitialAddUserValuesService

`InitialAddUserValuesService` tries to pre-fill `customer_number` and `contact_number` from the query parameters of the request and assigns the values to the `add_user` form array.

Service ID: `siso_customer_center.initial_add_user_values_service`

## InitialEditUserValuesService

`InitialEditUserValuesService` loads content from an User Content item and assigns the values to the `edit_user` form array.

Service ID: `siso_customer_center.initial_edit_user_values_service`
