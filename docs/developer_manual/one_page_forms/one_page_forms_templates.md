# One-page forms templates

### Templates list

|Path `Silversolutions/Bundle/EshopBundle/Resources/views/Forms/*`|Description|
|--- |--- |
|register_business.html.twig|Renders the business registration form|
|register_private.html.twig|Renders the private registration form|
|activate_business.html.twig|Renders the account activation form for existing customers|
|my_account.html.twig|Renders the update my account data form|
|buyer.html.twig|Renders the update the buyer address form|
|invoice.html.twig|Renders update the invoice address form|
|password_change.html.twig|Renders the change password form|
|password_reminder.html.twig|Renders the forgot password form|
|cancellation.html.twig|Renders the online cancellation form (RMA)|
|contact.html.twig|Renders the contact us form|

|Subtemplates|Description|
|--- |--- |
|party.html.twig|Renders the party form attributes|
|custom_form_label.html.twig|Renders custom template for the form label|


### Related custom Twig modifiers/functions/etc/

``` html+twig
# define a route to your form, pass the formTypeResolver as a parameter
<a href="{{ path('silversolutions_service', {'formTypeResolver': 'registration_private'}) }}">{{ 'msg.register_here'|st_translate }}</a>
```

### Related routes:

Alternative routes for the `FormsController::formsAction`

``` yaml
silversolutions_forms:
    path:  /register/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }

silversolutions_profile:
    path:  /profile/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }

silversolutions_password_reminder:
    path:  /password/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }

silversolutions_service:
    path:  /service/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }
```
