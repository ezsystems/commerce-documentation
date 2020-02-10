# Lost orders 

## Sending lost orders from Backend

If sending of an order to ERP fails, order is still stored in a database with a special state: *ordered\_failed*.  In the admin user interface the Lost Orders can be found in the tab eCommerce/Order Management. Filter by "Order failed".

![](attachments/23560385/23569081.png)

### There are several actions, that can be executed with lost orders:

<table>
<thead>
<tr class="header">
<th><pre><code>action</code></pre></th>
<th><pre><code>meaning</code></pre></th>
<th><pre><code>result</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>transfer to ERP</code></pre></td>
<td>lost order will be resend to ERP</td>
<td><p>user see error or success message depending if the lost order could be resend to ERP or not</p>
<p>if the sending of lost order failed, admin will get an email</p>
<p>if the sending of lost order was successfull customer who released this order will get an confirmation email</p></td>
</tr>
<tr>
<td><pre><code>remove lost order</code></pre></td>
<td>the <strong>lost order will be not removed</strong>, just the state of lost order will be changed to 'confirmed'</td>
<td>the lost order does not appear in the list anymore</td>
</tr>
</tbody>
</table>

# Technical implementation

### LostOrderService

This service is responsible to handle all lost order features

LostOrderService.php

    namespace: Siso\Bundle\CheckoutBundle\Service

    service id: siso_checkout.lost_order_service

#### Service methods:

<table>
<thead>
<tr class="header">
<th><pre><code>method</code></pre></th>
<th><pre><code>meaning</code></pre></th>
<th><pre><code>parameters</code></pre></th>
<th><pre><code>returns</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>createFailedOrderList</code></pre></td>
<td><pre><code>creates a list of lost orders</code></pre></td>
<td><br />
</td>
<td><pre><code>Basket[]</code></pre></td>
</tr>
</tbody>
</table>

### LostOrderController

This controller handles all lost order actions

<table>
<thead>
<tr class="header">
<th><pre><code>action</code></pre></th>
<th><pre><code>parameters</code></pre></th>
<th><pre><code>meaning</code></pre></th>
<th><pre><code>policy</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>processLostOrderAction</code></pre></td>
<td><pre><code>$basketId</code></pre></td>
<td><pre><code>send lost order to ERP</code></pre></td>
<td><pre><code>siso_policy/lostorder_process</code></pre></td>
</tr>
<tr>
<td><pre><code>deleteLostOrderAction</code></pre></td>
<td><pre><code>$basketId</code></pre></td>
<td><pre><code>set the state of lost order to &#39;confirmed&#39;</code></pre></td>
<td><pre><code>siso_policy/lostorder_process</code></pre></td>
</tr>
</tbody>
</table>

###   
Email notifications

Every time and order cannot be placed shop administrator will get an email. The email can be defined in the configuration:

``` 
parameters:

    siso_core.default.ses_swiftmailer:
        .....
        lostOrderEmailReceiver: %ses_eshop.lostorder_email%
```

Here is how the email looks like:

![](attachments/23560385/23569083.png)

**Templates are located here:**

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Emails/NotificationMail_FailedOrder.html.twig
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Emails/NotificationMail_FailedOrder.txt.twig
```

# Lost Order Command

**Important\!** For the CLI the host must be set, otherwise the urls and assets are not correct. Please make sure, you have this in your configuration.

``` 
# Please make sure, that you have this parameter set in your project configuration
# Set the host for your project here - depending on siteaccess

parameters:
    siso_core.default.host: localhost
```

Another possibility is to resend the lost orders via comman line tool:

**silversolutions:lostorder:process \[id\]**

``` 
php bin/console silversolutions:lostorder:process --help

Usage:
 silversolutions:lostorder:process [id]

Arguments:
 id                    basket id of the lost order

Options:
 --help (-h)           Display this help message.
 --quiet (-q)          Do not output any message.
 --verbose (-v|vv|vvv) Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
 --version (-V)        Display this application version.
 --ansi                Force ANSI output.
 --no-ansi             Disable ANSI output.
 --no-interaction (-n) Do not ask any interactive question.
 --shell (-s)          Launch the shell.
 --process-isolation   Launch commands from shell as a separate process.
 --env (-e)            The Environment name. (default: "dev")
 --no-debug            Switches off debug mode.
 --siteaccess          SiteAccess to use for operations. If not provided, default siteaccess will be used

Help:
 The siso_checkout:lostorder:process command sends all lost orders to ERP one after each other.
 You can specify the basket id of the lost order by yourself if you want to send only one lost order.
```

The behavior is the same as if sending lost orders via backend. If the sending of lost order fails, admin will get an email. If the sending was succesfull, customer who released this order will get an confirmation email.

Please notice, that in this context (command line tool) there is no request. Because of this no images embedded in email will be send, because it is not possible to generate the img path.

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-11-28 um 07.45.20.png](attachments/23560385/23563486.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screen Shot 2014-12-19 at 09.28.21.png](attachments/23560385/23563430.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order Management1.png](attachments/23560385/23569098.png) (image/png)  
![](images/icons/bullet_blue.gif) [failed\_order\_email\_admin 1.png](attachments/23560385/23569083.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order Management2.png](attachments/23560385/23569095.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order Management1.png](attachments/23560385/23569081.png) (image/png)  
