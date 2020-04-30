# Customer center - user actions

## User actions

There are several actions, that can be performed by the main contact from the user management entry page. The main contact can:

- activate/deactivate the user
- edit user
- add existing ERP contacts to the shop
- create new user in shop

## Activate/deactivate the user

The main contact can activate/deactivate the user account. If the user account was deactivated, the respective user can not login to the shop anymore. If the account has been activated again, the user can login to the shop, using the former login name and password.

![](../../../img/customer_center_user_actions_1.png)

## Edit, add and request new user

There are default forms prepared for these actions, that can be partially modified in the configuration.

Almost everything about the forms can be configured. There are two types of form fields:

- **static fields** - can not be removed from the form, some settings (like validation or name) can be configured.
- **dynamic fields** - can be removed or added, will be read/stored in/from eZ if corresponding eZ field identifier exists in the User class and can be sent to ERP (request new user).

### Form fields configuration

- you can change the form field type: http://symfony.com/doc/current/reference/forms/types.html - this will change the way how the field is rendered
- you can change the form field options, these are depending on the field type, that you have chosen.

#### Example Add user

??? note

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

These fields are considered to be *static* and must not be removed from the form's *attributes* definition:

- salutation - Drop-down field with predefined choices.
- name - Free text field.
- email - Free text field with e-mail validation.
- submit - This is the submit button for every form.

Except for submit, all field's values will be stored in the eZ user object and can be edited in the eZ administration.

## How to set up a new field

All custom fields must be defined in the *dynamic* section of the form's *attributes*. These fields have to be added to the user's content type in the eZ administration and the field identifier must be exactly the same as the key in the configuration.

For a detailed description, please see [How to extend the customer center forms](../customer_center_cookbook.md)

## Feedback messages

Every form defines a message for a successful submission and for an occurred error during the form data processing. The configuration keys are *validMessage* and *invalidMessage*, respectively. It's best practice to define identifiers for [text modules](../../../translations/translations.md) and edit the feedback content in the eZ administration. The content of the *invalidMessage* should contain correction suggestions.

## Initial values

A form can define a service in the *initialFormValuesService* key, that pre-fills the form fields with specific values. For example, these values could be read from some external data source or eZ content. The configuration value must a valid service ID and the service's class must implement the [InitialFormValuesServiceInterface](../customer_center_api/initialformvaluesserviceinterface.md).

## Form processors

The form processors are services that perform some special actions after the form has been submitted. They are executed sequentially according the order of definition in the configuration.

You can specify the form processors for a form in the configuration by the their respective service IDs. The service classes of all form processors must implement the [FormProcessorInterface](../customer_center_api/formprocessorinterface.md).

``` yaml
formProcessors:
    - siso_customer_center.processor.create_contact_in_erp
    - siso_customer_center.processor.add_user_in_ez
    - siso_customer_center.processor.update_user_roles
```

### List of form processors in the standard setup

|Name|ID|Description|
|--- |--- |--- |
|ValidateCustomerCenterActivationDataProcessor|siso_customer_center.validate_customer_center_activation|checks if given user has customer center enabled|
|CustomerCenterCreateUserDataProcessor|siso_customer_center.customer_center_create_user|creates user and company object in customer center|
|AddUserInEzFormProcessor|siso_customer_center.processor.add_user_in_ez|creates a new eZ user content object out of the posted form data|
|CreateContactInErpProcessor|siso_customer_center.processor.create_contact_in_erp|performs "CreateContact" request to ERP and assigns the contact number to eZUser|
|StoreUserInEzFormProcessor|siso_customer_center.processor.store_user_form_in_ez|performs storage of changed data in eZ|
|UpdateUserRolesFormProcessor|siso_customer_center.processor.update_user_roles|changes user roles in eZ backend for posted data|
|SaveProfileInSessionProcessor|siso_customer_center.processor.save_profile_in_session|stores changed user data of the current user in the session|
|ValidateCustomerAndContactNumberProcessor|siso_customer_center.processor.validate_customer_and_contact_number|checks if given customer and contact number are correct|
