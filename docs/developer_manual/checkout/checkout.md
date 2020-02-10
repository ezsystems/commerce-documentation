#  Checkout 

**Introduction**

eZ Commerce provides a one page checkout. The number of steps varies.

If a user is not logged in he will see 2 more steps: one with a login form and a second one for the invoice address. Customers having a customer number will already foreared to the step 3. 

### Step 1 - Login form

![](attachments/23560414/23561584.png)

### Step 2 - Invoice form

A customer not having a customer number or a anoymous user will have to add the invoice address first:

![](attachments/23560414/23561588.png)

### Step 3 - Delivery address

A user without delivery address wir see an empty delivery form. He can also chose the invoice address.

![](attachments/23560414/23561560.png)  

A customer with addresses can choose an address from a list:

![](attachments/23560414/23561578.png)

### Step 4 - Delivery and payment

![](attachments/23560414/23561580.png)

### Step 5 - Order summary

![](attachments/23560414/23561583.png)

## Important terms used in this document

ERP

**ERP** (Enterprise resource planning) is business [management](http://en.wikipedia.org/wiki/Management "Management") software—typically a suite of integrated applications—that a company can use to collect, store, manage and interpret data from many business activities, including:

  - [Product planning](http://en.wikipedia.org/wiki/Product_planning "Product planning"), cost
  - [Manufacturing](http://en.wikipedia.org/wiki/Manufacturing "Manufacturing") or service delivery
  - [Marketing](http://en.wikipedia.org/wiki/Marketing "Marketing") and sales
  - [Inventory management](http://en.wikipedia.org/wiki/Inventory_management "Inventory management")
  - Shipping and payment

*Source*: Wikipedia

Known ERP software packages:

  - Microsoft Dynamics NAV (former navision)
  - Microsoft Dynamics AX  
    
  - SAP

## Before you start 

Please keep in mind that Checkout is really connected with a lot of different modules in our shop. You will find more about the chekcout in these documents:

  - [General logic in checkout](General-logic-in-checkout_23560637.html)
  - Used forms in the checkout:  [Forms](Forms_23560915.html)
  - How are orders send to the ERP: [Order Submission](Order-Submission_23560252.html)

## How to do things after an order has been placed

you can define an EventListener which is triggered when an order has been placed.

``` 
<service id="ezcommerce_demo.confirmation_listener" class="%ezcommerce_demo.confirmation_listener.class%">
    <argument type="service" id="silver_basket.basket_repository" />
    <argument type="service" id="silver_trans.translator" />
    <argument type="service" id="siso_tools.mailer_helper" />
    <argument type="service" id="silver_catalog.data_provider_service" />
    <argument type="collection">
        <argument key="addresses">$siso_core.default.ses_swiftmailer$</argument>
        <argument key="subject">$order_confirmation.subject;siso_checkout$</argument>
    </argument>

    <tag name="kernel.event_listener" event="silver_eshop.response_message" method="onOrderResponse" priority="-10" />
    <tag name="kernel.event_listener"  event="silver_eshop.exception_message" method="onExceptionMessage" />

</service>
```

There are 2 tags involved: one for eZ Commerce which does not have an ERP system connected and one for the Advanced version with ERP

``` 
/**
 * Listens to the MessageExceptionEvent event.
 * The appropriate exception was thrown by the previous listener ("sendMessage" of the "AbstractMessageTransport")
 * This function:
 * - checks for downloads and send an email to the customer 
 *
 * @param MessageExceptionEvent $messageExceptionEvent
 * @return void
 *
 */
public function onExceptionMessage(MessageExceptionEvent $messageExceptionEvent)
{
    $message = $messageExceptionEvent->getMessage();
    $exception = $messageExceptionEvent->getException();
    if ($message instanceof CreateSalesOrderMessage && $exception instanceof LocalOrderRequiredException) {
        /** @var Order $requestDocument */
        $requestDocument = $message->getRequestDocument();

        /** @var Basket $basket */
        $basket = $this->basketRepository->findOneBy(array('guid' => $requestDocument->UUID->value));

        $this->checkForDownloads($basket);
    }
}
/**
 * This method must registered as a listener to the
 * 'silver_eshop.response_message' event. It will act in case an ERP is in place
 *
 * @param MessageResponseEvent $event
 */
public function onOrderResponse(MessageResponseEvent $event)
{
    $message = $event->getMessage();
    if ($message instanceof CreateSalesOrderMessage) {

        /** @var Order $requestDoc */
        $requestDoc = $message->getRequestDocument();

        $basket = $this->basketRepository->findOneBy(array('guid' => $requestDoc->UUID->value));
        $this->checkForDownloads($basket);
    }
}
```
[Important terms used in this document](#Checkout-Importanttermsusedinthisdocument)

[Before you start ](#Checkout-Beforeyoustart)

[How to do things after an order has been placed](#Checkout-Howtodothingsafteranorderhasbeenplaced)

[FAQ](#Checkout-FAQ)

[Templating](#Checkout-Templating)

[Cookbook](#Checkout-Cookbook)

[API](#Checkout-API)

## FAQ

Due to a restriction in JmsPayment Bundle there is a restriction that an order exeeding 99999.99999 causes an error message. Is it possible to extend this limit?

Yes it is\! There is an official workaround for the JMSPaymentCoreBundle: <http://jmspaymentcorebundle.readthedocs.io/en/latest/guides/overriding_entity_mapping.html>

Also check [Payment FAQ](https://doc.silver-eshop.de/display/EZC14/Payment+-+FAQ) for more details.

Is it possible to show the invoice form even if a customer has a customer number?

Yes it is.

Please check the documention here: [AjaxCheckoutController](https://doc.silver-eshop.de/display/EZC14/AjaxCheckoutController)

How can i use the "store delivery address" information in the order?

When a customer selects the checkbox "Store address" in the checkout  (step = delivery address) the a flag is stored in the basket and order.

It can be mapped in the xslt:

``` 
<Create_Shipping_Address><xsl:value-of select="Delivery/DeliveryParty/SesExtension/store" /></Create_Shipping_Address>
```

How can i add new options for payment?

How can i add more options for delivery?

How can the subject of the confirmation mail be changed (For shop owner and sales contact)?

This translations can be edited in order to change the subject of the order confirmation mails:

<table>
<tbody>
<tr>
<td><pre><code>siso_core.default.shop_owner_mail_subject: &quot;common.shop_owner_mail_subject&quot;</code></pre></td>
</tr>
<tr>
<td><pre><code>siso_core.default.sales_contact_mail_subject: &quot;common.sales_contact_mail_subject&quot;</code></pre></td>
</tr>
</tbody>
</table>

## Templating

The Checkout is using a list of templates which can be overridden if required. 

[Read more...](Checkout---Templates_23560337.html)

## Cookbook

Find recipes in [cookbook...](Checkout---Cookbook_23560340.html)

## API

See [Services for Checkout Forms](Services-for-Checkout-Forms_23560638.html)

## Attachments:

![](images/icons/bullet_blue.gif) [image2016-4-7 17:43:54.png](attachments/23560414/23561584.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-4-7 17:46:5.png](attachments/23560414/23561586.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-4-7 17:47:12.png](attachments/23560414/23561588.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-4-7 17:48:0.png](attachments/23560414/23561560.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-4-7 17:50:43.png](attachments/23560414/23561578.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-4-7 17:51:11.png](attachments/23560414/23561580.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-4-7 17:51:42.png](attachments/23560414/23561583.png) (image/png)  
