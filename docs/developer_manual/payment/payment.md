# Payment

The eZ Commerce ships with a payment bundle. This bundle depends on the [JMSPaymentCoreBundle](http://jmsyst.com/bundles/JMSPaymentCoreBundle). Thus, this third party bundle must be [installed manually](http://jmsyst.com/bundles/JMSPaymentCoreBundle/master/installation) if it wasn't already due to composer dependencies.

The eZ Commerce supports the following Payment providers:

<table>
<thead>
<tr class="header">
<th>Payment provider</th>
<th>Details</th>
<th>Code used in Basket</th>
</tr>
</thead>
<tbody>
<tr>
<td>Paypal Express</td>
<td>See <a href="PayPal_23561073.html">Paypal</a></td>
<td>paypal_express_checkout</td>
</tr>
<tr>
<td><div class="content-wrapper">
<img src="attachments/23560256/23563442.png" class="confluence-embedded-image" />
</td>
<td>See <a href="Telecash_23560650.html">Telecash</a></td>
<td>telecash_connect</td>
</tr>
<tr>
<td><div class="content-wrapper">
<img src="attachments/23560256/23563455.png" class="confluence-embedded-image confluence-thumbnail" width="100" />
</td>
<td>See <a href="Ogone_23560639.html">Ogone</a></td>
<td>ogone_gateway</td>
</tr>
<tr>
<td>Invoice</td>
<td>Used for non electronic payments</td>
<td>invoice</td>
</tr>
</tbody>
</table>

## Important configuration

<table>
<tbody>
<tr>
<td><p><code class="sourceCode php">jms_payment_core:</code><br />
<code class="sourceCode php">    </code><code class="sourceCode php">encryption:</code><br />
<code class="sourceCode php">        </code><code class="sourceCode php">provider: defuse_php_encryption</code><br />
<code class="sourceCode php">        </code><code class="sourceCode php">secret: </code><code class="sourceCode php"><span class="st">&#39;def0000033444be8556a1bd7e347a240d19d34a078112f3f18bbb74cb4caeff9f9df7e2f1c86f32826b6b262360791264a0aeb851bdb999f2b882038448966b3b1d40a79&#39;</code><br />
<code class="sourceCode php">        </code><code class="sourceCode php">enabled: <span class="kw">true</code></p></td>
</tr>
</tbody>
</table>

**Important: The secret has to be created by a command:**

|                                                            |
| ---------------------------------------------------------- |
| `php vendor/defuse/php-encryption/bin/generate-defuse-key` |

## Common Payment Workflow

The diagram shows the processes involved in a payment process.

When a user accepts the payment and the payment provider has acknowledged the data the

  - user will be redirected to the shop and 
  - in parallel the payment provider informs the shop calling a notify url of the shop ("Notify Shop") . This call is a direct call from the payment system and the shop 

![](attachments/23560256/23562134.png)

## Attachments:

![](images/icons/bullet_blue.gif) [telecash-logo.png](attachments/23560256/23563442.png) (image/png)  
![](images/icons/bullet_blue.gif) [ogone-logo.png](attachments/23560256/23563455.png) (image/png)  
![](images/icons/bullet_blue.gif) [removeme](attachments/23560256/23563457) (application/drawio)  
![](images/icons/bullet_blue.gif) [removeme.png](attachments/23560256/23563453.png) (image/png)  
![](images/icons/bullet_blue.gif) [Payment Process.xml](attachments/23560256/23563454.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Payment Process.xml.png](attachments/23560256/23563451.png) (image/png)  
![](images/icons/bullet_blue.gif) [Payment.xml](attachments/23560256/23563450.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Payment.xml.png](attachments/23560256/23563398.png) (image/png)  
![](images/icons/bullet_blue.gif) [Payment.xml](attachments/23560256/23563342.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Payment.xml.png](attachments/23560256/23563343.png) (image/png)  
![](images/icons/bullet_blue.gif) [Payment.xml](attachments/23560256/23563309.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Payment.xml.png](attachments/23560256/23563307.png) (image/png)  
![](images/icons/bullet_blue.gif) [Payment.xml](attachments/23560256/23563311.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Payment.xml.png](attachments/23560256/23563312.png) (image/png)  
![](images/icons/bullet_blue.gif) [Payment.xml](attachments/23560256/23563452.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Payment.xml.png](attachments/23560256/23563401.png) (image/png)  
![](images/icons/bullet_blue.gif) [Basket state flow standard.png](attachments/23560256/23562134.png) (image/png)  
