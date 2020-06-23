# FormProcessorInterface

`FormProcessorInterface` must be implemented by every service that is defined in the `formProcessors` key of a form definition.
For example, in `CustomerCenterBundle/Resources/config/customercenter.yml`:

``` yaml
siso_customer_center.default.form.add_user:
    formProcessors:
        - siso_customer_center.processor.validate_customer_and_contact_number
        # ...
```

|Method|Description|
|--- |--- |
|`execute(Form $form, array $lastResult = array())`|Processes the given form data and must return the `$lastResult` parameter with possible changes/additions|

## CheckUserEmailFormProcessor

`CheckUserEmailFormProcessor` checks if a User with given email address already exists in the company structure.  

Service ID: `siso_customer_center.processor.check_user_email`

## AddUserInEzFormProcessor

`AddUserInEzFormProcessor` creates a new User Content item out of the posted form data.
Besides the User Content Type's basic fields, any additional data is stored within the customer profile data.

Service ID: `siso_customer_center.processor.add_user_in_ez`

## CreateContactInErpProcessor

If `CreateContactInErpProcessor` is active, it sends the `CreateContact` request to the ERP.
This processor only creates the contact in the ERP system and sets `CUSTOMER_NUMBER` and `CONTACT_NUMBER` within the `$lastResult` array.
If further processing is necessary, subsequent processors must take care of it.

Service ID: `siso_customer_center.processor.create_contact_in_erp`

## SaveProfileInSessionProcessor

`SaveProfileInSessionProcessor` saves the `CustomerProfileData` in session if it's the current user and was defined by a previous form processor.

Service ID: `siso_customer_center.processor.save_profile_in_session`

## StoreUserInEzFormProcessor

`StoreUserInEzFor` processor stores the changed data.

Service ID: `siso_customer_center.processor.store_user_form_in_ez`

## UpdateUserRolesFormProcessor

`UpdateUserRolesFormProcessor` updates user Roles.

Service ID: `siso_customer_center.processor.update_user_roles`

## ValidateCustomerAndContactNumberProcessor

`ValidateCustomerAndContactNumberProcessor` checks given customer and contact number in ERP for validity and adds both to the `$lastResponse` array.
`FormDataProcessorException` is thrown in case of any failed validation. In that case no subsequent processor is reached.

Service ID: `siso_customer_center.processor.validate_customer_and_contact_number`
