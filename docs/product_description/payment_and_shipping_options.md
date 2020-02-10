#  Payment and shipping options 

<table>
<thead>
<tr class="header">
<th>Feature</th>
<th>Details</th>
</tr>
</thead>
<tbody>
<tr>
<td>Configuration of payment and shipment options</td>
<td><div class="content-wrapper">
<p>Next to standard payment method invoice an interface to Paypal Express is offered.</p>
<p>Payment providers Ogone/Ingenico and Telecash can be implemented on request.</p>
<p> The payment is based on a standard Symfony Bundle. An eZ Partner can integrate other payment providers. <br />
</p>
<p><img src="attachments/23561072/23563078.png" class="confluence-embedded-image" height="400" /><br />
</p>
<p>The payment and shipping method offered can be enabled or disabled per siteaccess.</p>
<p>A more complex logic can be implemented by partners. </p>
</td>
</tr>
<tr>
<td>Shipping costs</td>
<td><div class="content-wrapper">
<p>eZ Commerce and eZ Commerce Advanced offer a flexible way to define shipping costs, if this is not set in the ERP system.</p>
<p>Shipping costs can be setup per</p>
<ul>
<li>shop (eZ Commerce offers one shop only)</li>
<li>currency</li>
<li>shipping method</li>
<li>country</li>
<li>state</li>
<li>zip code (e.g. for exceptions such as delivery to islands)</li>
</ul>
<p>In addition it is possible to configure different shipping cost depending on the amount of the basket (including free of freight rules) :</p>
<p>If no shipping costs are defined for a given country, shipping method, currency and value of goods a fallback cost from the configuration is used.</p>
<p><img src="attachments/23561072/23568384.png" class="confluence-embedded-image" height="400" /><br />
</p>
</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-5-16\_11-9-4.png](attachments/23561072/23571100.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-5-31\_18-21-23.png](attachments/23561072/23563082.png) (image/png)  
![](images/icons/bullet_blue.gif) [shipping costs.png](attachments/23561072/23563084.png) (image/png)  
![](images/icons/bullet_blue.gif) [payment.png](attachments/23561072/23563083.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-5-31\_19-50-35.png](attachments/23561072/23563078.png) (image/png)  
![](images/icons/bullet_blue.gif) [shipping\_costs.png](attachments/23561072/23568384.png) (image/png)  
