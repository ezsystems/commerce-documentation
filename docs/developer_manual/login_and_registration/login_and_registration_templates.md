# Login and registration templates

## Templates list:
| Path     | Description        |
| -------- | ------------------ |
| Silversolutions/Bundle/EshopBundle/Resources/views/Forms/register\_private.html.twig  | Form for private customer registration |
| Silversolutions/Bundle/EshopBundle/Resources/views/Forms/register\_business.html.twig | Form for business customer registration  |
| Silversolutions/Bundle/EshopBundle/Resources/views/Forms/register\_choice.html.twig   | Overview page for registration, which offers buttons for the different registration types (and activation of existing customers) |
| Silversolutions/Bundle/EshopBundle/Resources/views/Forms/activate\_business.html.twig | Form for activation of existing customers   |
| Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout\_login.html.twig   | Login form in checkout   |
| Silversolutions/Bundle/EshopBundle/Resources/views/Security/login.html.twig   | Login form  |

## Related routes

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/ses_routing.yml`:

``` yaml
silversolutions_forms_user_choice:
    pattern:  /registration/choice
    defaults: { _controller: SilversolutionsEshopBundle:Forms:chooseUserType }
silversolutions_forms:
    pattern:  /register/{formTypeResolver}
    defaults:
        _controller: SilversolutionsEshopBundle:Forms:forms
        breadcrumb_path: silversolutions_forms_user_choice/silversolutions_forms
        breadcrumb_names: silversolutions_forms_user_choice|breadcrumb/common.silver_forms.$formTypeResolver$
logout:
    path:   /logout

login:
    path:   /login
    defaults:  { _controller: SilversolutionsEshopBundle:Security:login }
```
