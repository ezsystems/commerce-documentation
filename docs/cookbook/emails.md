# Emails

## Configuration

By default eZ Commerce uses [SwiftMailer](http://symfony.com/doc/master/reference/configuration/swiftmailer.html) in order to send the emails. This minimum configuration is necessary:

``` yaml
swiftmailer:
    transport:  smtp
    encryption: ssl
    auth_mode:  login
    host:       %ses_eshop.mail.smpt.host%
    username:   shopuser
    password:   shoptest
    #if you enable this parameter, the %ses_eshop.mail.master_address% will get every outgoing email from the shop!
    delivery_address: %ses_eshop.mail.master_address%
```

See [Configuration](../guide/configuration/configuration.md#settings-for-emails) for information on how to configure email addresses.

## Emails sent to the user

### Registration - private customer

Receiver: User who registers

Handled by `SendConfirmationMailDataProcessor`

Template:

```
SilversolutionsEshopBundle:Emails:ConfirmationMail_RegisterPrivate.html.twig
SilversolutionsEshopBundle:Emails:ConfirmationMail_RegisterPrivate.txt.twig
```

#### Double opt-in email for newsletter subscription

Receiver: User who requests the newsletter

Template:

```
Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_SubscribeNewsletter.html.twig
Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_SubscribeNewsletter.txt.twig
```

### Forgot password

Receiver: User who executes the "forgot password" function

Handled by `SendPasswordReminderEmailDataProcessor`

Template:

```
SilversolutionsEshopBundle:Emails:password_reminder.html.twig
SilversolutionsEshopBundle:Emails:password_reminder.txt.twig
```

## Emails sent to the user and to the admins

### Contact form

Receiver:

- `contactMailReceiver`
- User who sends out the contact form (if "send to my address" is checked)

Configuration:

``` yaml
parameters:
    siso_core.default.ses_swiftmailer:
        contactMailReceiver: azh@silversolutions.de
```

Handled by `SendContactEmailDataProcessor`

Template:

```
SilversolutionsEshopBundle:Emails:contact.html.twig
SilversolutionsEshopBundle:Emails:contact.txt.twig
```

### Checkout - order confirmation

Receiver:

- User who makes an order
- If customer center is active and approver triggers the order, approver and buyer get the email
- Sales person, if configured

Configuration:

``` yaml
parameters:
    #possible mode: config or customer, if you want to send 
    #email to the sales person, set to config and add 
    #sales_email_address
    siso_checkout.default.order_confirmation.sales_email_mode:  
    customer
    siso_checkout.default.order_confirmation.sales_email_address:
    
    #configure email subject
    siso_checkout.order_confirmation.subject: 
    "Order confirmation"
```

Handled by `OrderConfirmationListener`

Template:

```
SilversolutionsEshopBundle:Checkout/Email:order_confirmation.html.twig
SilversolutionsEshopBundle:Checkout/Email:order_confirmation.txt.twig
```

### Lost orders

Receiver:

- User who makes an order
- If customer center is active and approver triggers the order, approver and buyer get the email
- Sales person, if configured

Configuration:

``` yaml
parameters:
    #possible mode: config or customer, if you want to send 
    #email to the sales person, set to config and add 
    #sales_email_address
    siso_checkout.default.order_confirmation.sales_email_mode:  
    customer
    siso_checkout.default.order_confirmation.sales_email_address:
    
    #configure email subject
    siso_checkout.order_confirmation.subject: 
    "Order confirmation"
```

## Emails sent to shop admin

### Registration - business customer

Receiver: Administrator

Handled by `SendConfirmationMailDataProcessor`

Template:

```
SilversolutionsEshopBundle:Emails:ConfirmationMail_RegisterBusiness.html.twig
SilversolutionsEshopBundle:Emails:ConfirmationMail_RegisterBusiness.txt.twig
```

### Cancellation form

Receiver: Administrator

Configuration:

``` yaml
parameters:
    siso_core.default.ses_swiftmailer:
        cancellationMailReceiver: azh@silversolutions.de
```

Handled by `SendCancellationEmailDataProcessor`

Template:

```
Silversolutions/Bundle/EshopBundle/Resources/views/Emails/cancellation.html.twig
Silversolutions/Bundle/EshopBundle/Resources/views/Emails/cancellation.txt.twig
```

### Edit profile - buyer address

Receiver: Administrator

Configuration:

``` yaml
parameters:    
    siso_core.default.ses_swiftmailer:      
        mailReceiver: azh@silversolutions.de
```

Handled by `SendConfirmationMailDataProcessor`

Template:

```
SilversolutionsEshopBundle:Emails:ConfirmationMail_Buyer.html.twig
SilversolutionsEshopBundle:Emails:ConfirmationMail_Buyer.txt.twig
```

### Edit profile - invoice address

Receiver: Administrator

Configuration:

``` yaml
parameters:    
    siso_core.default.ses_swiftmailer:      
        mailReceiver: azh@silversolutions.de
```

Handled by `SendConfirmationMailDataProcessor`

Template:

```
Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_Address.html.twig
Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_Address.txt.twig
```

### Checkout - order failed

Receiver: Administrator

Configuration:

``` yaml
parameters:
    siso_core.default.ses_swiftmailer:  
        mailReceiver: azh@silversolutions.de

    #configure email subject
    siso_eshop.default.order_failed_subject: 
        "Order ERP submission failed"
```

Handled by `OrderFailedNotifyListener`

Template:

```
SilversolutionsEshopBundle:Emails:NotificationMail_FailedOrder.html.twig
SilversolutionsEshopBundle:Emails:NotificationMail_FailedOrder.txt.twig
```

## Fallback

### Fallback email

Sometimes when the email template cannot be found, the fallback email template is used instead.
This is the case for all emails, where the `SendConfirmationMailDataProcessor` is used.

Handled by `SendConfirmationMailDataProcessor`

Template:

```
SilversolutionsEshopBundle:Emails:ConfirmationMail_Fallback.html.twig
SilversolutionsEshopBundle:Emails:ConfirmationMail_Fallback.txt.twig
```

## Customer center

### Approve basket

Receivers: All approvers (or main contacts, if no approvers configured) from the company.

Configuration:

``` yaml
parameters:
    siso_customer_center.approve_basket.subject: 
    'Basket to approve'
```

Handled by `ApprovalController:sendToApproverAction`

Template:

```
SisoCustomerCenterBundle:CustomerCenter/Emails:approve_basket.html.twig
SisoCustomerCenterBundle:CustomerCenter/Emails:approve_basket.txt.twig
```

### Reject basket

Receiver: Buyer whose basket was rejected.

Configuration:

``` yaml
parameters:
    siso_customer_center.reject_basket.subject: 
    'Basket was rejected'
```

Handled by `ApprovalController:confirmRejectAction`

Template:

```
SisoCustomerCenterBundle:CustomerCenter/Emails:reject_basket.html.twig
SisoCustomerCenterBundle:CustomerCenter/Emails:reject_basket.txt.twig
```

### New account

Receiver: User whose account was created

Configuration:

``` yaml
parameters:
    siso_customer_center.password_mail.subject: 
    'Your new account'
```

Handled by `AddUserInEzFormProcessor`

Template:

```
SisoCustomerCenterBundle:CustomerCenter/Emails:new_account_pw.html.twig
SisoCustomerCenterBundle:CustomerCenter/Emails:new_account_pw.txt.twig
```

!!! note

    If no email templates can be rendered (either HTML and Twig), then this email is sent to the client.
