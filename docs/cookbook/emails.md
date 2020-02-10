#  Emails 

Here you can find a list with emails that will be sent out from shop and processes behind them. Also the general configuration can be found here.

## Configuration

By default eZ Commerce is using [SwiftMailer](http://symfony.com/doc/master/reference/configuration/swiftmailer.html) in order to send the emails. This minimum configuration is necessary:

``` 
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

These parameters are used to configure the outgoing emails:

**General e-mail configuration - emails.yml**

``` 
parameters:
    siso_core.default.ses_swiftmailer:
        #general mail sender
        mailSender: noreply@silversolutions.de
        #administrator mail receiver
        mailReceiver: azh@silversolutions.de
        #mail receiver for contact form
        contactMailReceiver: azh@silversolutions.de
        #mail receiver for lost order
        lostOrderEmailReceiver: contact@silversolutions.de
        #mail receiver for cancellation form
        cancellationMailReceiver: contact@silversolutions.de
        #mail receiver for shop notifications
        shopOwnerMailReceiver: azh@silversolutions.de

    #possible mode: config or customer - if you want to send email to the sales person, set to config and add sales_email_address       
    siso_checkout.default.order_confirmation.sales_email_mode: customer
    siso_checkout.default.order_confirmation.sales_email_address:

    #configure email subject
    siso_checkout.default.order_confirmation_subject: "Order confirmation"    
    siso_eshop.default.order_failed_subject: "Order ERP submission failed"
    siso_core.default.cancellation_subject: common.cancellation_email_subject
```

**Customer center**

``` 
parameters:
    siso_customer_center.approve_basket.subject: 'Basket to approve'
    siso_customer_center.reject_basket.subject: 'Basket was rejected'
    siso_customer_center.password_mail.subject: 'Your new account'
```

## Email list and processes

<table>
<thead>
<tr class="header">
<th>Process</th>
<th>Receiver</th>
<th>How to reproduce</th>
<th>Configuration</th>
<th>Responsible Service</th>
<th>Template</th>
<th>Screenshot</th>
</tr>
</thead>
<tbody>
<tr>
<td>Emails send to the user</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Registration - private customer</code></pre></td>
<td><p>User who registered</p></td>
<td><div class="content-wrapper">
<ul>
<li>Fill the form data and send</li>
<li>(<em>register/private</em>)</li>
</ul>

<p>Liebe/r Jonas Schock</p>
<p><br />
</p>
<ul>
<li>salutation possible</li>
</ul>
</td>
<td><p><br />
</p>
<br />

<p><br />
</p></td>
<td><pre><code>SendConfirmationMailDataProcessor</code></pre></td>
<td><p><code>SilversolutionsEshopBundle:Emails:ConfirmationMail_RegisterPrivate.html.twig</code></p>
<p><code>SilversolutionsEshopBundle:Emails:ConfirmationMail_RegisterPrivate.txt.twig</code></p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<p>User:</p>
<p><img src="attachments/23560607/23563478.png" class="confluence-embedded-image" height="250" /></p>
</td>
</tr>
<tr>
<td><code>DOI email for subscribe newsletter</code></td>
<td><p>User who requested the newsletter</p></td>
<td><div class="content-wrapper">
<ul>
<li>Input the email address and send</li>
</ul>

<p>Liebe/r {email}</p>
<p><br />
</p>
<ul>
<li>salutation and name not possible, since we are using data from the form</li>
</ul>
</td>
<td><br />
</td>
<td><br />
</td>
<td><p><code>Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_SubscribeNewsletter.html.twig</code></p>
<p><code>Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_SubscribeNewsletter.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Forgot password</code></pre></td>
<td><p>User who executed 'forgot password' function</p></td>
<td><div class="content-wrapper">
<ul>
<li>Fill the form data and send</li>
<li>(<em>password/reminder</em>)</li>
</ul>

<p>Liebe/r {email}</p>
<p><br />
</p>
<ul>
<li>salutation and name not possible, since we only have the email address from the form</li>
</ul>
</td>
<td><p><br />
</p>
<br />

<p><br />
</p></td>
<td><pre><code>SendPasswordReminderEmailDataProcessor</code></pre></td>
<td><p><code>SilversolutionsEshopBundle:Emails:password_reminder.html.twig</code></p>
<p><code>SilversolutionsEshopBundle:Emails:password_reminder.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td>Emails send to the user and to the admins</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Contact form</code></pre></td>
<td><ol>
<li><pre><code>contactMailReceiver</code></pre></li>
<li>User who send out the contact form (if checked send to my address)</li>
</ol>
<br />

<p><br />
</p></td>
<td><div class="content-wrapper">
<ul>
<li><p>Fill the form data and send</p></li>
<li><p><em>(<em>service/contact</em>)</em></p></li>
</ul>

<p>Liebe/r {email}</p>
<p><br />
</p>
<ul>
<li>salutation not possible, since we are using the data from the form</li>
<li>name and last would be possible</li>
</ul>
</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    siso_core.default.ses_swiftmailer:
        contactMailReceiver: azh@silversolutions.de</code></pre>
</td>
<td><pre><code>SendContactEmailDataProcessor</code></pre></td>
<td><p><code>SilversolutionsEshopBundle:Emails:contact.html.twig</code></p>
<p><code>SilversolutionsEshopBundle:Emails:contact.txt.twig</code></p></td>
<td><div class="content-wrapper">
<p>User:</p>
<p><img src="attachments/23560607/23563244.png" class="confluence-embedded-image" height="250" /></p>
</td>
</tr>
<tr>
<td><pre><code>Checkout - order confirmation</code></pre></td>
<td><ol>
<li>User who ordered</li>
<li>If customer center is active and approver triggered the order, approver and buyer will get the email</li>
<li>Sales person, if configured<br />
<br />
</li>
</ol>
<br />

<p><br />
</p></td>
<td><div class="content-wrapper">
<p>If the order process was succesfull, user will get an confirmation email</p>
<ul>
<li>Ensure that there is connection to ERP</li>
<li>order something</li>
</ul>

<p>Liebe/r {email}</p>
<p><br />
</p>
<ul>
<li>salutation and user name not possible, since we are using the data from the form. In the checkout forms we only have<br />
Name/CompanyName</li>
</ul>
</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    #possible mode: config or customer, if you want to send 
    #email to the sales person, set to config and add 
    #sales_email_address
    siso_checkout.default.order_confirmation.sales_email_mode:  
    customer
    siso_checkout.default.order_confirmation.sales_email_address:
    
    #configure email subject
    siso_checkout.order_confirmation.subject: 
    &quot;Order confirmation&quot;</code></pre>
<p><br />
</p>
<p><br />
</p>
</td>
<td><pre><code>OrderConfirmationListener</code></pre></td>
<td><p><code>SilversolutionsEshopBundle:Checkout/Email:order_confirmation.html.twig</code></p>
<p><code>SilversolutionsEshopBundle:Checkout/Email:order_confirmation.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Lost orders</code></pre></td>
<td><ol>
<li>User who ordered</li>
<li>If customer center is active and approver triggered the order, approver and buyer will get the email</li>
<li>Sales person, if configured<br />
<br />
</li>
</ol>
<br />

<p><br />
</p>
<p><br />
</p></td>
<td><p>Same behavior as in the checkout process:</p>
<ul>
<li>If the lost order process was succesfull, user will get an confirmation email</li>
<li>If it was not possible to send an order, admin will get a lost order email</li>
</ul></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    #possible mode: config or customer, if you want to send 
    #email to the sales person, set to config and add 
    #sales_email_address
    siso_checkout.default.order_confirmation.sales_email_mode:  
    customer
    siso_checkout.default.order_confirmation.sales_email_address:
    
    #configure email subject
    siso_checkout.order_confirmation.subject: 
    &quot;Order confirmation&quot;</code></pre>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>Emails send to shop admin</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Registration - business customer</code></pre></td>
<td><p>Admin</p></td>
<td><ul>
<li>Fill the form data and send</li>
<li>(<em>register/business</em>)</li>
</ul></td>
<td><p><br />
</p>
<br />

<p><br />
</p></td>
<td><pre><code>SendConfirmationMailDataProcessor</code></pre></td>
<td><p><code>SilversolutionsEshopBundle:Emails:ConfirmationMail_RegisterBusiness.html.twig</code></p>
<p><code>SilversolutionsEshopBundle:Emails:ConfirmationMail_RegisterBusiness.txt.twig</code></p></td>
<td><div class="content-wrapper">
<p>Administrator:</p>
<p><img src="attachments/23560607/23563446.png" class="confluence-embedded-image" height="250" /></p>
<p><br />
</p>
</td>
</tr>
<tr>
<td>Cancellation form</td>
<td><ol>
<li>Administrator</li>
</ol></td>
<td><ul>
<li><p>Fill the form data and send</p></li>
<li><p><em>(<em>service/cancellation</em>)</em></p></li>
</ul></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    siso_core.default.ses_swiftmailer:
        cancellationMailReceiver: azh@silversolutions.de</code></pre>
<p><br />
</p>
<p><br />
</p>
<p><br />
</p>
</td>
<td><pre><code>SendCancellationEmailDataProcessor</code></pre></td>
<td><p><code>Silversolutions/Bundle/EshopBundle/Resources/views/Emails/cancellation.html.twig</code></p>
<p><code>Silversolutions/Bundle/EshopBundle/Resources/views/Emails/cancellation.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Edit profile - buyer address</code></pre></td>
<td><ol>
<li>Administrator</li>
</ol>
<br />

<p><br />
</p></td>
<td><ul>
<li>Fill the form data and send</li>
<li>Only for customers with customer number</li>
<li>(<em>profile/buyer</em>)</li>
</ul></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:    
    siso_core.default.ses_swiftmailer:      
        mailReceiver: azh@silversolutions.de</code></pre>
</td>
<td><pre><code>SendConfirmationMailDataProcessor</code></pre></td>
<td><p><code>SilversolutionsEshopBundle:Emails:ConfirmationMail_Buyer.html.twig</code></p>
<p><code>SilversolutionsEshopBundle:Emails:ConfirmationMail_Buyer.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td>Edit profile - invoice address</td>
<td><ol>
<li>Administrator</li>
</ol></td>
<td><ul>
<li>Fill the form data and send</li>
<li>Only for customers with customer number</li>
<li>(<em>profile/invoice</em>)</li>
</ul></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:    
    siso_core.default.ses_swiftmailer:      
        mailReceiver: azh@silversolutions.de</code></pre>
</td>
<td><pre><code>SendConfirmationMailDataProcessor</code></pre></td>
<td><p><code>Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_Address.html.twig</code></p>
<p><code>Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_Address.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Checkout - order failed</code></pre></td>
<td><ol>
<li>Administrator</li>
</ol>
<br />

<p><br />
</p></td>
<td><p>If it was not possible to send an order, admin will get a lost order email</p>
<ul>
<li>Ensure that there is no connection to ERP</li>
<li>Order something</li>
</ul></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    siso_core.default.ses_swiftmailer:  
        mailReceiver: azh@silversolutions.de

    #configure email subject
    siso_eshop.default.order_failed_subject: 
        &quot;Order ERP submission failed&quot;</code></pre>
</td>
<td><pre><code>OrderFailedNotifyListener</code></pre></td>
<td><p><code>SilversolutionsEshopBundle:Emails:NotificationMail_FailedOrder.html.twig</code></p>
<p><code>SilversolutionsEshopBundle:Emails:NotificationMail_FailedOrder.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td>Fallback</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Fallback email</code></pre></td>
<td><br />
</td>
<td><div class="content-wrapper">

<p>Sometimes when the email template can not be found, fallback email template is used instead.</p>
<p>This is the case for all emails, where the SendConfirmationMailDataProcessor is used.</p>
</td>
<td><br />
</td>
<td><pre><code>SendConfirmationMailDataProcessor</code></pre></td>
<td><p><code>SilversolutionsEshopBundle:Emails:ConfirmationMail_Fallback.html.twig</code></p>
<p><code>SilversolutionsEshopBundle:Emails:ConfirmationMail_Fallback.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td>Customer center</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Approve basket</code></pre></td>
<td><ul>
<li>All approvers (or main contacts, if no approvers configured) from the company</li>
</ul></td>
<td><ul>
<li>Try to order with a buyer who exceeded his budget.</li>
<li>In the checkout summary page click on 'Send to approver' button</li>
<li>All approvers from the company will get an email</li>
</ul></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    siso_customer_center.approve_basket.subject: 
    &#39;Basket to approve&#39;</code></pre>
</td>
<td><pre><code>ApprovalController:sendToApproverAction</code></pre></td>
<td><p><code>SisoCustomerCenterBundle:CustomerCenter/Emails:approve_basket.html.twig</code></p>
<p><code>SisoCustomerCenterBundle:CustomerCenter/Emails:approve_basket.txt.twig</code></p></td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Reject basket</code></pre></td>
<td><ul>
<li>Buyer whose basket was rejected</li>
</ul></td>
<td><ul>
<li>Login as an approver</li>
<li>Go the the page with a list of baskets waiting for approvement</li>
<li>reject one basket</li>
<li>Buyer whose basket was rejected will get an email</li>
</ul></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    siso_customer_center.reject_basket.subject: 
    &#39;Basket was rejected&#39;</code></pre>
</td>
<td><pre><code> ApprovalController:confirmRejectAction</code></pre></td>
<td><p><code>SisoCustomerCenterBundle:CustomerCenter/Emails:reject_basket.html.twig</code></p>
<p><code>SisoCustomerCenterBundle:CustomerCenter/Emails:reject_basket.txt.twig</code></p>
<pre><code></code></pre>
<p><br />
</p></td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>New account</code></pre></td>
<td><ul>
<li>User whose account was created</li>
</ul></td>
<td><ul>
<li>Login as main contact</li>
<li>Request/add new user</li>
<li>User whose account was created will get an email</li>
</ul></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>parameters:
    siso_customer_center.password_mail.subject: 
    &#39;Your new account&#39;</code></pre>
</td>
<td><pre><code>AddUserInEzFormProcessor</code></pre></td>
<td><p><code>SisoCustomerCenterBundle:CustomerCenter/Emails:new_account_pw.html.twig</code></p>
<p><code>SisoCustomerCenterBundle:CustomerCenter/Emails:new_account_pw.txt.twig</code></p></td>
<td><br />
</td>
</tr>
</tbody>
</table>

If for some reason no email templates can be rendered (both html and twig), then this email will not be sent to the client.

## Attachments:

![](images/icons/bullet_blue.gif) [image2016-1-20 13:25:27.png](attachments/23560607/23563244.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-1-22 12:41:11.png](attachments/23560607/23563478.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-1-25 12:9:29.png](attachments/23560607/23563446.png) (image/png)  
