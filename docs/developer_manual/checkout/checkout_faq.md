# Checkout FAQ

## Due to a restriction in JmsPayment Bundle there is a restriction that an order exeeding 99999.99999 causes an error message. Is it possible to extend this limit?

Yes it is. There is an [official workaround for the JMSPaymentCoreBundle.](http://jmspaymentcorebundle.readthedocs.io/en/latest/guides/overriding_entity_mapping.html)

Also check [Payment FAQ](Payment---FAQ_23560268.html) for more details.

## Is it possible to show the invoice form even if a customer has a customer number?

Yes it is.

Please check the documention here: [AjaxCheckoutController](AjaxCheckoutController_23560323.html)

## How can i use the "store delivery address" information in the order?

When a customer selects the checkbox "Store address" in the checkout  (step = delivery address) the a flag is stored in the basket and order.

It can be mapped in the xslt:

``` 
<Create_Shipping_Address><xsl:value-of select="Delivery/DeliveryParty/SesExtension/store" /></Create_Shipping_Address>
```

## How can the subject of the confirmation mail be changed (For shop owner and sales contact)?

This translations can be edited in order to change the subject of the order confirmation mails:

```
siso_core.default.shop_owner_mail_subject: "common.shop_owner_mail_subject"
siso_core.default.sales_contact_mail_subject: "common.sales_contact_mail_subject"
```
