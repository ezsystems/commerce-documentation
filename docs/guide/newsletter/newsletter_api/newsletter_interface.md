# Newsletter interface

eZ Commerce provides a common interface for newsletter providers. If another newsletter provider will be implemented the following methods has to be implemented.

The newsletter interface is stored in: `Siso\Bundle\NewsletterBundle\Api\NewsletterInterface`

## subscribeNewsletter

`public function subscribeNewsletter(array $params, CustomerProfileData $customerProfileData = null);`

Subscribes newsletter with given $params. Any data, that was e.g. posted from a form, can be passed.
If $customerProfileData was given, attributes from $customerProfileData are used to subscribe the newsletter
otherwise the required attributes must be specified in $params.

Returns a list with the newsletter ids and the status if the action was successful (true/false).

## unsubscribeNewsletter

`public function unsubscribeNewsletter(array $params, CustomerProfileData $customerProfileData = null);`

Unsubscribes newsletter with given $params. Any data, that was e.g. posted from a form, can be passed.
If $customerProfileData was given, attributes from $customerProfileData are used to unsubscribe the newsletter
otherwise the required attributes must be specified in $params.

Returns a list with the newsletter ids and the status if the action was successful (true/false).

## updateNewsletter

`public function updateNewsletter(array $params, CustomerProfileData $customerProfileData = null);`

Updates newsletter details with given $params.
The implementation of this method MUST dispatch UpdateNewsletterEvent

Returns a list with the newsletter ids and the status if the action was successful (true/false).

## updateUserEmail

`public function updateUserEmail($oldEmail, $newEmail);`

Updates newsletter users email address

Returns a list with the newsletter ids and the status if the action was successful (true/false).

## getEmail

`public function getEmail(array $params, CustomerProfileData $customerProfileData = null);`

Returns the correct email address

- if CustomerProfileData is provided, the email from SesUser has to be used
- if CustomerProfileData is not provided, the email from params has to be used

## doesUserExistInNewsletter

`public function doesUserExistInNewsletter(array $params, CustomerProfileData $customerProfileData = null);`

Returns true, if user exists in the newsletter provider.
This method just checks, if user is known to the newsletter provider, but does not evaluate the status
(if user subscribes newsletter).

The goal of this method is to find out, if user exists in the DB of the newsletter provider (active/inactive),
because even if the user is inactive, he probably already confirmed his email address in the past
(e.g. via double-opt-in process)

## doesUserSubscribeNewsletter

`public function doesUserSubscribeNewsletter(array $params, CustomerProfileData $customerProfileData = null);`

Reads the user newsletter status - if user subscribes newsletter - from the provider.
If $customerProfileData was given, attributes from $customerProfileData are used to check the newsletter status
otherwise the required attributes must be specified in $params.

Returns a list with the newsletter ids and the status if user subscribes newsletter (true/false).

## doesUserSubscribeAtLeastOneNewsletter

`public function doesUserSubscribeAtLeastOneNewsletter(array $newsletterStatusList);`

Returns true if user subscribes at least one newsletter.
No call to API should be done here. Instead the result of one methods above should be passed as parameter.

## getSubscribedNewsletterIds

`public function getSubscribedNewsletterIds(array $newsletterStatusList);`

Returns a list of newsletter ids that user subscribes.
No call to API should be done here. Instead the result of one methods above should be passed as parameter.
