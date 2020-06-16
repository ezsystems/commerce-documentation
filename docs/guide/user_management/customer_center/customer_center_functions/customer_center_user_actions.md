# Customer center user actions

## Edit, add and request new user

There are default forms prepared for the actions of editing users that can be partially modified in the configuration.

There are two types of form fields:

- static fields - cannot be removed from the form, some settings (like validation or name) can be configured.
- dynamic fields - can be removed or added, are read/stored in the content if corresponding content Field identifier exists in the User Content Type and can be sent to ERP (request new user).

### Form field configuration

You can change the [form field type.](http://symfony.com/doc/3.4/reference/forms/types.html)
This changes the way the field is rendered.
You can change the form field options. They depend on the field type.

For example:

``` yaml
siso_customer_center.default.form.add_user:
        invalidMessage: error_message_customer_center_add_user
        validMessage: success_message_customer_center_add_user
        initialFormValuesService: siso_customer_center.initial_add_user_values_service
        formProcessors:
            - siso_customer_center.processor.add_user_in_ez
        #form attributes, define a list here
        #the options type and options are required!
        attributes:
            #the static attributes can not be removed from the form, you can simply configure the settings and constraints here
            static:
                salutation:
                    #valid symfony form type
                    type: choice
                    #options for the choosen symfony type
                    options:
                        choices:
                            'mr': 'Mr'
                            'mrs': 'Mrs'
                        empty_value: '--- please choose ---'
                        constraints:
                            Symfony\Component\Validator\Constraints\NotBlank:
                name:
                    type: text
                    options:
                        constraints:
                            Symfony\Component\Validator\Constraints\NotBlank:
                            Symfony\Component\Validator\Constraints\Length:
                                min: 1
                                max: 50
                email:
                    type: email
                    options:
                        constraints:
                            Symfony\Component\Validator\Constraints\NotBlank:
                            Silversolutions\Bundle\EshopBundle\Entities\Forms\Constraints\Email:
                submit:
                    type: submit
                    options:
            dynamic:
                budget_order:
                    type: money
                    options:
                        empty_data: 0
                budget_month:
                    type: money
                    options:
                        empty_data: 0
                customer_number:
                    type: text
                    options:
                        read_only: true
                contact_number:
                    type: text
                    options:
                        read_only: true
```

## Predefined fields

These fields are considered to be static and must not be removed from the form's attribute definition:

- `salutation` - drop-down field with predefined choices
- `name` - free text field
- `email` - free text field with email validation
- `submit` - submit button for the form

Except for `submit`, all field values are stored in the User Content item and can be edited in the Back Office.

## Setting up a new field

All custom fields must be defined in the `dynamic` section of the form's attributes.
These fields have to be added to the User Content Type in the Back Office and the field identifier must be the same as the key in the configuration.

For a detailed description, see [Extending the Customer center forms](../customer_center_cookbook.md#extending-the-customer-center-forms).

## Feedback messages

Every form defines a message for successful submission and for an error that can occur during form data processing.
The configuration keys are `validMessage` and `invalidMessage`, respectively.
It's best practice to define identifiers for [text modules](../../../translations/translations.md)
and edit the feedback content in the Back Office.
The content of `invalidMessage` should contain correction suggestions.

## Initial values

A form can define a service in the `initialFormValuesService` key that pre-fills the form fields with specific values.
These values can be read from an external data source or from content.
The configuration value must be a valid service ID and the service's class must implement [`InitialFormValuesServiceInterface`](../customer_center_api/initialformvaluesserviceinterface.md).

## Form processors

The form processors are services that perform special actions after the form is submitted.
They are executed sequentially according to the order of definition in the configuration.

You can specify the form processors for a form in the configuration by the their respective service IDs.
The service classes of all form processors must implement [`FormProcessorInterface`](../customer_center_api/formprocessorinterface.md).

``` yaml
formProcessors:
    - siso_customer_center.processor.create_contact_in_erp
    - siso_customer_center.processor.add_user_in_ez
    - siso_customer_center.processor.update_user_roles
```

### List of form processors in the standard setup

|Name|ID|Description|
|--- |--- |--- |
|`ValidateCustomerCenterActivationDataProcessor`|`siso_customer_center.validate_customer_center_activation`|Checks if the given user has Customer center enabled|
|`CustomerCenterCreateUserDataProcessor`|`siso_customer_center.customer_center_create_user`|Creates user and Company Content item in Customer center|
|`AddUserInEzFormProcessor`|`siso_customer_center.processor.add_user_in_ez`|Creates a new User Content item out of the posted form data|
|`CreateContactInErpProcessor`|`siso_customer_center.processor.create_contact_in_erp`|Sends a `CreateContact` request to ERP and assigns the contact number to the user|
|`StoreUserInEzFormProcessor`|`siso_customer_center.processor.store_user_form_in_ez`|Stores changed data in content|
|`UpdateUserRolesFormProcessor`|`siso_customer_center.processor.update_user_roles`|Changes user Roles in for posted data|
|`SaveProfileInSessionProcessor`|`siso_customer_center.processor.save_profile_in_session`|Stores changed user data of the current user in the session|
|`ValidateCustomerAndContactNumberProcessor`|`siso_customer_center.processor.validate_customer_and_contact_number`|Checks if the given customer and contact number are correct|
