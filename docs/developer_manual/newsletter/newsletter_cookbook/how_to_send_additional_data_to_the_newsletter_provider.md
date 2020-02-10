#  How to send additional data to the Newsletter provider 

You need to implement an event listener, that will listen to the '*subscribe\_newsletter\_event*' or  '*update\_newsletter\_event*'.

``` 
public function setCustomParameters(SubscribeNewsletterEvent $event)
{
    $customerProfileData = $event->getCustomerProfileData();
    $params = $event->getParams();

    if ($customerProfileData instanceof CustomerProfileData && !array_key_exists('order_amount', $params)) {
        $userId = $customerProfileData->sesUser->sesUserObjectId;
        /** fetch the amount of all user orders */
        $orderAmount = $this->basketRepository->getUserOrdersAmount($userId);
        $params['order_amount'] = $orderAmount;
    }

    $event->setParams($params);
}
```

``` 
<service id="project.newsletter.subscribe_newsletter_listener" class="%project.newsletter.subscribe_newsletter_listener.class%">
    <argument type="service" id="project.basket_repository"/>  
    <tag name="kernel.event_listener" event="subscribe_newsletter_event" method="setCustomParameters" />
</service>
```

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2016-11-14 um 10.09.43.png](attachments/23560199/23570768.png) (image/png)  
![](images/icons/bullet_blue.gif) [amount.png](attachments/23560199/23570769.png) (image/png)  
![](images/icons/bullet_blue.gif) [new.png](attachments/23560199/23570770.png) (image/png)  
