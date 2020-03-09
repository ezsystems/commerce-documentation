# TokenController

**Route**:

`/token/[token]`

Example: `/token/124f564f6d4df4fd3fd4df34fd34fd`

TokenController will be called always to process a token, e.g. after user registration when the user clicks on the activation link.

The goal of this controller is to load the token from the database, activate the user account and invalidate the token.

Each token as has the following attributes to process the token-specific logic:

- $actionServiceId
- $actionServiceMethod
- $actionServiceMethodParameter

``` php
/** @var TokenServiceMethodProcessorService $tokenServiceMethodProcessor */
$tokenServiceMethodProcessor = $this->get('silver_eshop.token_service_method_processor');
//trigger the logic for this token with the service
$response = $tokenServiceMethodProcessor->$callableMethod(
       $tokenEntity,
       $callableService,
       $callableMethod
);
```

The service `silver_eshop.token_service_method_processor` implements the custom logic for each token and returns a Response.

To learn more about the TokenServiceMethodProcessor, see here:

-  [Token Service Method Processor](Token-Service-Method-Processor_23560590.html)
