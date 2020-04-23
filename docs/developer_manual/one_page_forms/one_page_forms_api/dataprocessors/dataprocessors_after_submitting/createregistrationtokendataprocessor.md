# CreateRegistrationTokenDataProcessor

The goal of this DataProcessor is to create a new Token with the Help of the [TokenService](../../../../../cookbook/token/tokenservice.md).

ID of this service:

`ses_forms.create_registration_token_data_processor`

The parameters for the method createToken() are taken from the configuration:

`forms.yml`:

``` yaml
ses_registration:
        #time in seconds how long the token is valid
        registration_token_valid_until: 7200
        registration_token_action_service: silver_forms.token.enable_ez_user
        registration_token_action_service_method: enableEzUser 
```
