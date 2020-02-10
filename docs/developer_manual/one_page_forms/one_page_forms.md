#  One-page forms 

## Introduction

eZ Commerce offers a common concept for one-page forms.

One-page forms have several common criteria:

  - They are displayed on a single page
  - After submitting some processes are executed in the backend
  - After server response user sees a confirmation page with a success or error messages.

If you have a usecase like that (e.g. registration form, contact form...) you are welcome to make a usage of the eZ Commerce one-page forms concept.

eZ Commerce uses the [Symfony forms](http://symfony.com/doc/current/forms.html) as a part of the solution, so you can profit of the Symfony forms benefits:

  - Easy handling with form entities
  - Simple and dynamic rendering with form types
  - Automatic forms validation
  - Usage of the validation groups and much more

Additionally you are able to pre-fill the form, when it is loaded for the first time. This gives you the opportunity e.g. to read some user data and improve the user experience by offering some values by default. The process that pre-fills the form is called ***preDataProcessor.***

You are also able to define processes that will be executed after the form was submitted. These processes are services that will be executed one after each other and they are able to exchange the data. These processes are called ***dataProcessors.***

If the process execution was not successfull, the process is able to store an error message, that will be displayed for the user on the confirmation page.

You can decide what should happend, after the form was submitted - on server response. You can differ between valid and invalid response and change the  behavior according your needs.

Usually the confirmation page will be displayed, but you can choose one of following behaviours (see configuration below):

  - confirmation page
  - redirect to another page:
      - another route from the shop
      - external url

-----

## How does it work?

Your task is to define and implement the form component parts (form entity, form type, template) and its services (preDataProcessor and dataProcessors) and bind everything within the [configuration](#One-pageforms-config).

The configuration parameter must follow this syntax:

``` 
# the last part of the parameter name - formTypeResolver - identifies your form 
# and has a special function, see below
ses_forms.configs.{formTypeResolver}
```

### FormTypeResolver

The {formTypeResolver} identifies your form in the url.

*FormsController::formsAction* is responsible to handle the form and {formTypeResolver} is the parameter, that tells him, which forms configuration should be used.

``` 
silversolutions_service:
    path:  /service/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }
```

That´s why when you call the url:

``` 
/service/registration_private
```

The *FormsController::formsAction* uses the forms configuration, that is mentioned in th example below.

#### Define another url for the form

If you want to change the url for your specific usecase, you can define a new route for the *FormsController::formsAction.*

The parameter {formTypeResolver} still must be part of the url.

``` 
test_project_forms:
    path:  /shop_functions/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }
```

Then by calling the url, the *FormsController::formsAction* will be used.

``` 
/shop_functions/registration_private
```

### <span id="One-pageforms-config" class="confluence-anchor-link">Configuration

By a simple configuration you are able to define the whole form behavior. See the full configuration example:

**$formTypeResolver = 'registration\_private'** Expand source 

``` 
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

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Configuration key</p>
<p>(* required)</p></th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>modelClass *</code></pre></td>
<td><p>Full qualified class path to the form entity.</p>
<p>This class needs to extend the class <strong>Silversolutions\Bundle\EshopBundle\Entities\Forms\AbstractFormEntity</strong></p></td>
</tr>
<tr>
<td><pre><code>typeClass (* if typeService not defined)</code></pre></td>
<td><p>Full qualified class path to the form type. Possible, but usage of</p>
<pre><code>typeService </code></pre>
<p>will give you more flexibility.</p></td>
</tr>
<tr>
<td><pre><code>typeService (* if typeClass not defined)</code></pre></td>
<td><p>ID of the form type service.</p></td>
</tr>
<tr>
<td><pre><code>template *</code></pre></td>
<td>Template that renders the form.</td>
</tr>
<tr>
<td><pre><code>validMessage</code></pre></td>
<td>General success message that will be displayed on the confirmation page, if the form submission was successful.</td>
</tr>
<tr>
<td><pre><code>invalidMessage</code></pre></td>
<td>General error message that will be displayed on the confirmation page, if the form submission failed.</td>
</tr>
<tr>
<td><pre><code>preDataProcessor</code></pre></td>
<td>ID of the service that pre-fills the form.</td>
</tr>
<tr>
<td><pre><code>dataProcessors</code></pre></td>
<td>List of service IDs, that will be executed one after each other after the form was submitted.</td>
</tr>
<tr>
<td><pre><code>response</code></pre></td>
<td><p>Behavior definition on the server response. If not defined, the confirmation page (template) is displayed.</p></td>
</tr>
<tr>
<td><pre><code>policy</code></pre></td>
<td><p>Defines the eZ user required policy. If user doesn´t have the policy, he will not be able to see the form.</p>
<p>The policy must be defined in following form</p>
<pre><code> &quot;module/function&quot;</code></pre></td>
</tr>
</tbody>
</table>

## Important terms used in this document

Symfony forms

**[Symfony forms](http://symfony.com/doc/current/forms.html)** that are handled in eZ Commerce consists by default of two parts:

  - Form entity
  - Form type

*Form entity* is just a simple class that holds the data and contains the [validation](https://symfony.com/doc/current/reference/constraints.html) annotations.

[*Form type*](https://symfony.com/doc/current/reference/forms/types.html#main) (usually defined as a service) builds the form. Here is defined which form attributes are rendered and how: e.g. as an input field, or textarea, custom attributes (css classes, data attributes) can be set here, you can define the labels or validation groups and much more.

The form itself is than build by the appropriate instance like that:

**Example**

``` 
class FormController extends Controller
{
    public function createFormAction()
    {
        $formType = $this->get('test_form_type_service');
        $formEntity = new TestForm();       
        
        $form = $this->createForm($formType, $formEntity);

        return $this->render('ProjectTestBundle:Form:create_form.html.twig', array('form' => $form->createView()));
    }
}
```

## reCAPTCHA

[EWZRecaptchaBundle](https://github.com/excelwebzone/EWZRecaptchaBundle) is used and provides easy reCAPTCHA form field for Symfony.

"reCAPTCHA is a free service that protects your website from spam and abuse.

Have a look at the reCAPTCHA implementation in the [cookbook](http://confluence.extranet.silversolutions.de:8090/display/EX/One-page+forms+-+Cookbook).

## FAQ

Can I change the min length for the phone validator?

 Yes, you can configure the min and max length for the [phone validator](https://doc.silver-eshop.de/display/EZC14/One-page+forms+-+API).

``` 
ses_phone_validator:
    min: 9
    max: 15
```

Which forms are prepared for reCAPTCHA?

  - Contact form
  - Cancellation form
  - Registration forms
      - private
      - business
  - Activate business

Where can the reCAPTCHA be activated for forms?

The reCAPTCHA can be activated in the Configuration Settings of eZ Commerce.

 ![](plugins/servlet/confluence/placeholder/unknown-attachment "image2017-1-18 9:31:40.png")

What can be configured at the moment?

It is possbile to configure the following options in the "Configuration settings":

  - theme (light or dark)
  - type (audio or image)
  - size (normal or compact)
  - defer (When present, it specifies that the script is executed when the page has finished parsing.)
  - async (When present, it specifies that the script will be executed asynchronously as soon as it is available.)

reCaptcha confirms that I'm not a robot without assigning a task, why?

Google’s risk analysis is sure that you’re human, in this case you won't be bothered with a reCaptcha task.

Quote from Google

The new reCAPTCHA is here. A significant number of your users can now attest they are human without having to solve a CAPTCHA. Instead with just a single click they’ll confirm they are not a robot. We’re calling it the No CAPTCHA reCAPTCHA experience.

## Templating

The existing eZ Commerce one-page forms are using a list of templates which can be overridden if required. 

[Read more.](One-page-forms---Templates_23560838.html)

## Cookbook

Find recipes in the [cookbook.](One-page-forms---Cookbook_23560845.html)

## API

Read more about the ***preDataProcessor***, ***dataProcessors*** and the forms validation in the [API chapter](One-page-forms---API_23560842.html).
