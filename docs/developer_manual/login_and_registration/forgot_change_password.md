#  Forgot / Change password 

## Goal

This functionality allows customer to set new password. There are 2 different scenarios which are described below.

## Forgot password

If the customer does not remember the old password and cannot login to the shop, he can use the forgot password page.

![](attachments/23560942/23563261.png)

#### Workflow

1.  The customer needs to provide the login or email address to which the link for password-resetting will be sent.  
    Additionally, if the shop expects the customer number as login-name, the customer can specify the customer number - it will search through the users inside the customer center.

2.  If the user exists, an email with an unique URL will be sent. It includes the correct token.

3.  When clicked, the user is redirected to a page where he can reset the password.

    Only if the token is provided and user is anonymous (i.e. the browser has no logged-in session), he will be able to change password.

#### Implementation

**route**

``` 
silversolutions_password_reminder:
    pattern:  /password/{formTypeResolver}
    defaults:
        _controller: SilversolutionsEshopBundle:Forms:forms
        breadcrumb_path: silversolutions_password_reminder
        breadcrumb_names: common.silver_forms.$formTypeResolver$
```

Configuration for password reminder (entity, service, template and data processors)

``` 
ses_forms.configs.reminder:
    modelClass: Silversolutions\Bundle\EshopBundle\Form\PasswordReminder
    typeService: silver_forms.password_reminder_type
    template: SilversolutionsEshopBundle:Forms:password_reminder.html.twig
    invalidMessage: error_message_password_reminder
    validMessage: success_message_password_reminder
    dataProcessors:
        - ses_forms.password_reminder_data_processor
        - siso_core.data_processor.send_password_reminder_email
```

##### Service

When building a form the shop checks if login with customer number is enabled. If so, customerNumber field is added.

``` 
if($enableCustomerNumber) {
    $builder
        ->add('customerNumber', 'text', array(
            'required' => false,
            'label' => $this->transService->translate('Customer number (if known)') . ': '
        ));
}
```

##### Data Processors

1.  **ses\_forms.password\_reminder\_data\_processor **  
    This data processor checks if data provided in the form are correct and if the user exists.

2.  **siso\_core.data\_processor.send\_password\_reminder\_email**  
    If user exists email with link to change password functionality is sent. Link will be valid for a period of time specified in configuration

    ``` 
    siso_core.default.forget_password_token_valid: 1 hour
    ```

##### Using in template

``` 
 <a href="{{ path('silversolutions_password_reminder', {'formTypeResolver': 'reminder'}) }}" class="link">{{ 'Forgot your password?'|st_translate }}</a>
```

The standard feature of Eshop does respect the special case that a shop allows registrations of more than one account having the the same email address. This is useful when eZ Commerce has to run a B2B and a B2C shop in one installation.

## Change password

If the customer is logged in, but wants to change his password to new one (for security reasons), he can use the change password form, which is located in profile page.

![](attachments/23560942/23563262.png)

#### Workflow

1.  First action is to check if user is logged in and no token is provided. Otherwise the user cannot access this page.
2.  User fills the form with new password and click send.
3.  The form is validated and new password is set for customer.

#### Implementation

``` 
silversolutions_password_change:
    pattern:  /change_password/{token}
    defaults:
        _controller: SilversolutionsEshopBundle:CustomerProfileData:changePassword
        token: null
```

Configuration for password change (entity, service, template)

``` 
ses_forms.configs.password_change:
    modelClass: Silversolutions\Bundle\EshopBundle\Form\PasswordChange
    typeService: silver_forms.password_change_type
    template: SilversolutionsEshopBundle:Forms:password_change.html.twig
    invalidMessage: error_message_password_change
    validMessage: success_message_password_change
```

##### Action

First the action tries to get the correct user.

**CustomerProfileDataController -\> changePasswordAction**

``` 
if(!isset($token)) {
    if(!$eZHelperService->isEzUserLoggedIn()) {
        $params = array(
            'errorMsg' => $this->getTrans()->translate(self::ERROR_ACCESS_DENIED)
        );
    } else {
        $user = $eZHelperService->getCurrentUser();
    }
}
```

The we submit the form data and display result.

##### Using in template

``` 
 <a href="{{ path('silversolutions_password_change') }}" class="button float_left">{{ 'Change password'|st_translate() }}</a>         
```

## Attachments:

![](images/icons/bullet_blue.gif) [10891635\_10205344944601149\_1993480457514776306\_n.jpg](attachments/23560942/23563265.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-03-25 10:38:33.png](attachments/23560942/23563261.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-03-25 10:46:00.png](attachments/23560942/23563262.png) (image/png)  
