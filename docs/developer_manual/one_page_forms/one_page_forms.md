# One-page forms

## Introduction

eZ Commerce offers a common concept for one-page forms.

One-page forms have several common criteria:

- They are displayed on a single page
- After submitting some processes are executed in the backend
- After server response user sees a confirmation page with a success or error messages.

If you have a use case like that (e.g. registration form, contact form...) you are welcome to make a usage of the eZ Commerce one-page forms concept.

eZ Commerce uses the [Symfony forms](http://symfony.com/doc/current/forms.html) as a part of the solution, so you can profit of the Symfony forms benefits:

- Easy handling with form entities
- Simple and dynamic rendering with form types
- Automatic forms validation
- Usage of the validation groups and much more

Additionally you are able to pre-fill the form, when it is loaded for the first time. This gives you the opportunity e.g. to read some user data and improve the user experience by offering some values by default. The process that pre-fills the form is called `preDataProcessor`.

You are also able to define processes that will be executed after the form was submitted. These processes are services that will be executed one after each other and they are able to exchange the data. These processes are called `dataProcessors`.

If the process execution was not successfull, the process is able to store an error message, that will be displayed for the user on the confirmation page.

You can decide what should happend, after the form was submitted - on server response. You can differ between valid and invalid response and change the  behavior according your needs.

Usually the confirmation page will be displayed, but you can choose one of following behaviors (see configuration below):

- confirmation page
- redirect to another page:
  - another route from the shop
  - external url

## How does it work?

Your task is to define and implement the form component parts (form entity, form type, template) and its services (preDataProcessor and dataProcessors) and bind everything within the [configuration](#One-pageforms-config).

The configuration parameter must follow this syntax:

``` 
# the last part of the parameter name - formTypeResolver - identifies your form 
# and has a special function, see below
ses_forms.configs.{formTypeResolver}
```

### FormTypeResolver

The `{formTypeResolver}` identifies your form in the url.

`FormsController::formsAction` is responsible to handle the form and `{formTypeResolver}` is the parameter, that tells him, which forms configuration should be used.

``` yaml
silversolutions_service:
    path:  /service/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }
```

That's why when you call the url `/service/registration_private`.

The *FormsController::formsAction* uses the forms configuration, that is mentioned in th example below.

#### Define another url for the form

If you want to change the url for your specific usecase, you can define a new route for the *FormsController::formsAction.*

The parameter {formTypeResolver} still must be part of the url.

``` yaml
test_project_forms:
    path:  /shop_functions/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }
```

Then by calling the url, the `FormsController::formsAction` will be used.

``` 
/shop_functions/registration_private
```

### Configuration

By a simple configuration you are able to define the whole form behavior. See the full configuration example:

`$formTypeResolver = 'registration_private'` 

``` yaml
parameters:
    ses_forms.configs.registration_private:
        modelClass: Silversolutions\Bundle\EshopBundle\Entities\Forms\RegisterPrivate
        # either typeClass or typeService has to be defined
        typeClass: Silversolutions\Bundle\EshopBundle\Entities\Forms\Types\RegisterPrivateType
        typeService: silver_forms.register_private_type
        template: SilversolutionsEshopBundle:Forms:register_private.html.twig
        # translation key should be defined here
        invalidMessage: error_message_register
        validMessage: success_register_private 
        policy: siso_policy/forms_profile_edit
        response:
            # behaviour on valid response
            valid:
                # renders a template - confirmation page
               template: SilversolutionsEshopBundle:Forms:register_private_valid.html.twig
                # redirect to external url              
                httpResponse: 'http://www.google.de'
                # redirect the another shop route
                routeName: 'silversolutions_forms_user_choice'
            # behaviour on invalid response         
            invalid:         
                template: SilversolutionsEshopBundle:Forms:register_private_valid.html.twig
                httpResponse: 'http://www.google.de'
                routeName: 'silversolutions_forms_user_choice'
        preDataProcessor: ses_forms.fill_private_form
        dataProcessors:            
            - ses_forms.create_ez_user
            - ses_forms.disable_ez_user
            - ses_forms.create_registration_token_data_processor
            - ses_forms.send_confirmation_data_processor
```

|Configuration key (* required)|Description|
|--- |--- |
|modelClass *|Full qualified class path to the form entity. This class needs to extend the class Silversolutions\Bundle\EshopBundle\Entities\Forms\AbstractFormEntity|
|typeClass (* if typeService not defined)|Full qualified class path to the form type. Possible, but usage of `typeService` will give you more flexibility.|
|typeService (* if typeClass not defined)|ID of the form type service.|
|template *|Template that renders the form.|
|validMessage|General success message that will be displayed on the confirmation page, if the form submission was successful.|
|invalidMessage|General error message that will be displayed on the confirmation page, if the form submission failed.|
|preDataProcessor|ID of the service that pre-fills the form.|
|dataProcessors|List of service IDs, that will be executed one after each other after the form was submitted.|
|response|Behavior definition on the server response. If not defined, the confirmation page (template) is displayed.|
|policy|Defines the eZ user required policy. If user doesn't have the policy, he will not be able to see the form. The policy must be defined in following form "module/function"|

## reCAPTCHA

[EWZRecaptchaBundle](https://github.com/excelwebzone/EWZRecaptchaBundle) is used and provides easy reCAPTCHA form field for Symfony.

reCAPTCHA is a free service that protects your website from spam and abuse.

Have a look at the reCAPTCHA implementation in the [cookbook](one_page_forms_cookbook.md).
