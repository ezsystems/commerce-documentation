# Newsletter2Go Service

This is the concrete implementation of the NewsletterInterface for the Newsletter2Go provider.  
Newsletter2Go uses the REST approach to connect to the API.

[Read more](https://docs.newsletter2go.com/?_ga=1.186190697.1183183675.1471410241#/) about the API Documentation.

Service ID

`siso_newsletter.newsletter.newsletter2go_service`

## Configuration

Following attributes have to be configured in order to connect to Newsletter2Go via API  

``` yaml
siso_newsletter.default.newsletter2go_username: '%newsletter2go_username%'
siso_newsletter.default.newsletter2go_password: '%newsletter2go_password%'
siso_newsletter.default.newsletter2go_auth_key: '%newsletter2go_auth_key%'
siso_newsletter.default.newsletter2go_ssl_verification: false
```

You will find the authentication data in the Newsletter2Go backend:

![](../../../img/newsletter2go_service_1.png)

![](../../../img/newsletter2go_service_2.png)

## How the service connects to the API?

Internally the Newsletter2GoService makes a usage of the Newsletter2GoApiService, that can be used, if new methods needs to be implemented in order to communicate with the Newsletter2Go provider via the API.

``` php
public function connectApi($path, $method, $data = array());
public function getApiFormattedDate(\DateTime $dateTime);
public function getApiGenderCode($genderCode);
```

## Send additional data to Newsletter2Go

### SubscribeNewsletterEvent

Newsletter2GoService dispatch an event, before user is created in the address book.

**SubscribeNewsletterEvent is dispatched**

``` 
...
//dispatch event before new recipient is created
$event = new SubscribeNewsletterEvent(
    self::RECIPIENT_PATH,
    NewsletterConstants::METHOD_POST,
    $params,
    $customerProfileData
);
$this->eventDispatcher->dispatch(
    NewsletterEvents::SUBSCRIBE_NEWSLETTER_EVENT,
    $event
);

$response = $this->newsletter2GoApiService->connectApi(
    self::RECIPIENT_PATH,
    NewsletterConstants::METHOD_POST,
    $event->getParams()
);
...
```

### SubscribeNewsletterEvent listener

Every listener, that listen to this event is able to add additional data, that will be send to the Newsletter2Go provider. However, if the attribute does not exist in the provider yet, it has to be created first in order to be stored.

**Example SubscribeNewsletterEvent listener**

``` 
public function setCustomParameters(SubscribeNewsletterEvent $event)
{
    $params = $event->getParams();
    $customerProfileData = $event->getCustomerProfileData();
 
    if (!array_key_exists('blocked', $params)) {
        $params['blocked'] = $customerProfileData->sesUser->contact->isBlocked;
    }

    $event->setParams($params);
}
```

``` 
<service id="custom_listener" class="%custom_listener.class%">
    <tag name="kernel.event_listener" event="subscribe_newsletter_event" method="setCustomParameters" />
</service>
```

### UpdateNewsletterEvent

Newsletter2GoService dispatch an event, before user is updated.

**SubscribeNewsletterEvent is dispatched**

``` 
...
//dispatch event before recipient is updated
$event = new UpdateNewsletterEvent(
    self::RECIPIENT_PATH,
    NewsletterConstants::METHOD_POST,
    $params,
    $customerProfileData
);
$this->eventDispatcher->dispatch(
    NewsletterEvents::UPDATE_NEWSLETTER_EVENT,
    $event
);

$response = $this->newsletter2GoApiService->connectApi(
    self::RECIPIENT_PATH,
    NewsletterConstants::METHOD_POST,
    $event->getParams()
);
...
```

### UpdateNewsletterEvent listener

Every listener, that listen to this event is able to add additional data, that will be send to the Newsletter2Go provider. However, if the attribute does not exist in the provider yet, it has to be created first in order to be stored.

**Example UpdateNewsletterEvent listener**

``` 
public function setCustomParameters(UpdateNewsletterEvent $event)
{
    $params = $event->getParams();
    $customerProfileData = $event->getCustomerProfileData();

    // sets customer number for logged user
    if ($customerProfileData instanceof CustomerProfileData) {
        $customerNumber = $customerProfileData->sesUser->customerNumber;
        if (isset($customerNumber) && !empty($customerNumber)) {
            $params['customer_number'] = $customerNumber;
        }
    }

    $event->setParams($params);
}
```

``` 
<service id="custom_listener" class="%custom_listener.class%">
    <tag name="kernel.event_listener" event="update_newsletter_event" method="setCustomParameters" />
</service>
```
