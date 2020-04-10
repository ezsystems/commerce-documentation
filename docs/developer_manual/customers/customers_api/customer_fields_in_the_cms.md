# Customer fields in the CMS

## Content Type user in the CMS

By default the eZ User class contains following fields:

|Field|Field identifier|Datatype|Notice|
|--- |--- |--- |--- |
|Name|name|Text line|This attribute can not be removed from the User class.</br>There are users (like admin, editors) who don't have any customer_profile_data information.|
|First name|first_name|Text line||
|Last name|last_name|Text line||
|Salutation|salutation|SesSelection|The User salutation is stored here|
|User Account|user_account|User account||
|Signature|signature|Text block||
|Image|image|Image||
|Customer number|customer_number|Text line||
|Contact Number|contact_number|Text line||
|Customer Profile Data|customer_profile_data|sesprofiledata|This field contains a Base64 encoded string.</br>If decoded and unserialized, this results in a customer profile data model entity.|
|Budget per order|budget_order|Float||
|Budget per month|budget_month|Float||

## Access to the profile

Not every user is allowed to modify his profile data in the shop (e.g. guest). You have to add the policy `siso_policy/forms_profile_edit` explicit to a role and assign it to customers. The standard already provides a role "Ecommerce registered users" which includes this policy. 

### Getting the policy in the Controller

``` php
return $this->render(
            'SilversolutionsEshopBundle::details.html.twig',
            array(
                'grant_profile_edit' => $this->isGranted(new AuthorizationAttribute('siso_policy', 'forms_profile_edit')),
            )
        );
```

### Using the policy in the Template

``` html+twig
{% if grant_profile_edit %}
    <a href="{{ path('silversolutions_profile', {'formTypeResolver': 'my_account'}) }}" class="button float_left">{{ 'Change My Account'|st_translate('profile') }}</a>
    <div class="float_left">&nbsp;
    <a href="{{ path('ez_legacy', {'module_uri': '/user/password'}) }}" class="button float_left">{{ 'Change password'|st_translate() }}</a>
{% endif %}
```

### Prohibit access to the form

You can also prohibit access to the form if the user calls a specific URL, where he does not have required role. This is set in the configuration.

**forms.yml**

``` yaml
ses_forms.configs.buyer:
    ...
    policy: siso_policy/forms_profile_edit
    ... 
```

Example:

User calls URL `/profile/buyer` (wants to edit the buyer address) without appropriate role.
