# Token Service Method Processor

Inside the token controller, we have now a layer (this service) to implement the custom logic for each token.

For example, in the case for the registration token, it basically calls the method that is stored in the token attribute `$actionServiceMethod` to the service that is stored in the attribute `$actionServiceId`. In addition, it does some validation and returns a response object.

We know, this breaks some basic MCV logic, but in this case we can afford it since we are able to outsource custom logic for different tokens.

Please not, that the service in `$actionServiceId` and this service, the `TokenServiceMethodProcessorService`, both need to implement a method with the name of `$actionServiceMethod`. This method MUST return a response of type `Symfony\Component\HttpFoundation\Response`.
