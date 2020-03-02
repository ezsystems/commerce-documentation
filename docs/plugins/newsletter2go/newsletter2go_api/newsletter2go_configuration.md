# Newsletter2Go Configuration

See newsletter2go configuration (in newsletter.yml):

``` yaml
parameters:
    #******** configuration for newsletter2go provider ************#
    # authentication
    siso_newsletter.default.newsletter2go_username: '%newsletter2go_username%'
    siso_newsletter.default.newsletter2go_password: '%newsletter2go_password%'
    siso_newsletter.default.newsletter2go_auth_key: '%newsletter2go_auth_key%'
    siso_newsletter.default.newsletter2go_ssl_verification: false

    # list of shop supported salutations - will be translated to the correct newsletter2go salutation code
    siso_newsletter.default.newsletter2go_gender_female: ['mrs', 'Mrs.', 'f']
    siso_newsletter.default.newsletter2go_gender_male: ['mr', 'Mr.', 'm']

    # custom attributes must be defined in the newsletter2go provider first
    siso_newsletter.default.newsletter2go_custom_attribute_user_language: user_language
```
