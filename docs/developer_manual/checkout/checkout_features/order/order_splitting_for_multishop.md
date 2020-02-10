#  Order splitting for multishop 

If there are products from the MAIN shop in the basket, The order is sent to the main shop ERP, and the basket data is modified.

By default this possibility is disabled for the shop, but it is possible to modified this in the configuration: 

**LocalOrderManagementBundle/Resources/config/local\_order\_management.yml**

``` 
   #Enable or disable the possibility to split the order for multishops
    siso_local_order_management.default.order_splitting_for_multishop: false
```

For the implementation of this feature ***event listeners*** are used.

## Listeners: *onExceptionMessage*

In order to split the orders, the exception "onExceptionMessage" is catch, that will interrupt the default process (sending to ERP).

New listener is created:

**Siso/Bundle/LocalOrderManagementBundle/Resources/config/services.xml**

``` 
<parameter key="siso_local_order_management.order_splitting.class">Siso\Bundle\LocalOrderManagementBundle\EventListener\OrderSplittingListener</parameter>

<service id="siso_local_order_management.order_splitting" class="%siso_local_order_management.order_splitting.class%">
    <argument type="service" id="ezpublish.config.resolver"/>
    <argument type="service" id="silver_basket.basket_service" />
    <argument type="service" id="silver_basket.basket_repository" />
    <argument type="service" id="siso_tools.mailer_helper" />
    <argument type="service" id="siso_local_order_management.invoice_service" />
    <argument type="service" id="doctrine.orm.entity_manager" />
    <tag name="kernel.event_listener"  event="silver_eshop.exception_message" method="onExceptionMessage" />
</service>
```

Logic inside the new listener:

  - Checks if the order contains some products of the MAIN shop.
  - Creates a new order only with the products from the "MAIN" shop and data of the shop owner
  - Submits the new order to ERP

Some of the products contains a 'main\_erp' attribute set in the *"$remoteDataMap*". To get that information there is a new method inside the "OrderSplittingListener"

We use a constant called USE\_MAIN\_ERP\_FLAG to define the name for the attribute 'main\_erp'.  
**checkProductsInMainShop** Expand source 

``` 
/**
 * Checks the products from the basket.
 * If at least one product of the the basket belongs to main shop, returns true.
 *
 * @param Basket $basket
 * @return bool
 */
protected function checkProductsInMainShop(Basket $basket)
{
    $basketLines = $basket->getLines();
    /** @var BasketLine $basketLine */
    foreach ($basketLines as $basketLine) {
        $remoteDataMap = $basketLine->getRemoteDataMap();

        if (isset($remoteDataMap[self::USE_MAIN_ERP_FLAG]) && !empty($remoteDataMap[self::USE_MAIN_ERP_FLAG])){
            return true;
        }
    }

    return false;
}
```

In order to mark the products from the MAIN shop, a flag can be added in the template.  
``` 
<input type="hidden" name="ses_basket[0][main_erp]" value="1"/>
```

In order to create the new order with products from the main shop there are some conditions, see code below; 

**onExceptionMessage** Expand source 

``` 
public function onExceptionMessage(MessageExceptionEvent $messageExceptionEvent)
{
    $exception = $messageExceptionEvent->getException();
    $message = $messageExceptionEvent->getMessage();
    $orderSplittingForMultishop = $this->configResolver->getParameter(
        'order_splitting_for_multishop',
        'siso_local_order_management'
    );

    if ($message instanceof CreateSalesOrderMessage
        && $exception instanceof LocalOrderRequiredException && $orderSplittingForMultishop) {
            
        .....
    
        if ($basket instanceof Basket
            && $this->checkProductsInMainShop($basket)
        ) {
            ...
            $this->webConnectorErpService->submitOrder($copiedBasket);
            .... 
         }
    }
}
```

There is an special attribute that is also set in the DataMap, so it will be possible in the templates to difference if there is a order from the main shop. 

``` 
$copiedBasket->addToDataMap(true, self::MULTISHOP_ORDER);
```

And there is an special method to create the new  basket and the new response:

The basket items are only the ones marked with the main\_erp flag.

``` 
/**
 * Copy the current basket and change some information for the splitting order.
 * The copied basket will have the standard logic, send emails, lost orders...
 *
 * @param Basket $basket
 * @return Basket
 *
 */
protected function createMainShopBasket(Basket $basket)
{
    ...

    return $copiedBasket;
}
```

After create the response it sends the confirmation email but without show the price calculation.

``` 
{% if not multishop_order == true %}
    <th style="border-right: 1px solid #999999; border-bottom: 1px solid #999999; font-weight: bold; text-align: center">{{ 'Unit price'|st_translate(null, {}, null, siteaccess) }} {{ '%vat% VAT'|st_translate(null, {'%vat%': vat}, null, siteaccess) }}</th>
    <th style="border-bottom: 1px solid #999999; font-weight: bold; text-align: right">{{ 'Total price'|st_translate(null, {}, null, siteaccess) }}</th>
{% endif %}
```

## Internal Process and implementation

Set the configuration settings (Split orders)

``` 
siso_local_order_management.default.send_order_to_erp: false

siso_local_order_management.default.order_splitting_for_multishop: true
```

<table>
<thead>
<tr class="header">
<th>Buyer and Invoice Party</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td><p>The buyer party for the splited order is generated using the buyer party of shop owner taken from a ERP request.</p>
<p>In Multishop there is a configuration for the customer number of the shop owner.</p>
<pre><code>shop_owner_customer_number</code></pre>
<p>This value is the customer number set in ERP.</p>
<p>If there is no ERP connection or there is a temporary problem,</p>
<p>the invoice and buyer address are empty and only filled with the customer number.</p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>protected function createMainShopBasket(Basket $basket)
{
    $copiedBasket = $this-&gt;basketService-&gt;copyBasket($basket);

    $newParty= new Party();
    $shopOwnerCustomerNumber = $this-&gt;configResolver
        -&gt;getParameter(&#39;shop_owner_customer_number&#39;, &#39;siso_local_order_management&#39;);

    //get User information for customer number to fill the address data, because they are needed for the email
    $customerInformation = $this-&gt;webConnectorErpService-&gt;selectCustomer($shopOwnerCustomerNumber);
    if (isset($customerInformation) &amp;&amp; isset($customerInformation-&gt;BuyerCustomerParty-&gt;Party)){
        $buyerParty = $customerInformation-&gt;BuyerCustomerParty-&gt;Party;
    } else {
        $buyerParty = new Party();
        $partyIdentification = new PartyPartyIdentification();
        $partyIdentification-&gt;ID-&gt;value = $shopOwnerCustomerNumber;
        $buyerParty-&gt;PartyIdentification[0] = $partyIdentification;
    }
    $copiedBasket-&gt;setBuyerParty($buyerParty);
    $copiedBasket-&gt;setInvoiceParty($buyerParty);
</code></pre>
</td>
</tr>
</tbody>
</table>

**Emails**

These emails are send with the standard process in vendor/silversolutions/silver.e-shop/src/Siso/Bundle/CheckoutBundle/EventListener/OrderConfirmationListener.php in the methods onOrderResponse and sendMailToRecipient.

Both emails are based on the standard order confirmation email templates: 

'SilversolutionsEshopBundle:Checkout/Email:order\_confirmation.txt.twig' and 'SilversolutionsEshopBundle:Checkout/Email:order\_confirmation.html.twig'

<table style="width:100%;">
<colgroup>
<col style="width: 4%" />
<col style="width: 51%" />
<col style="width: 8%" />
<col style="width: 35%" />
</colgroup>
<thead>
<tr class="header">
<th>Receiver</th>
<th>Info</th>
<th>PDF</th>
<th>Code</th>
</tr>
</thead>
<tbody>
<tr>
<td>Owner of the shop</td>
<td><div class="content-wrapper">
<p>Email with the order which was send to ERP</p>
<p>These differences to the standard email are realized by checking the variable 'multishop_order', which is stored in the dataMap of the copied basket:</p>
<ul>
<li>a special intro text is set in the email "email_multishop_order_intro_text" (uses  TextModules from the BackEnd</li>
<li>the delivery and shipping information are hidden</li>
<li>the basket is displayed without prices</li>
<li><p>The image on the top (banner) is different and there is a new block in the template for the image</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{{ block(&#39;multishop_email_image&#39;) }}</code></pre>
<p>and it is possible to override it in:</p>
<strong>EshopBundle/Resources/views/Emails/blocks.html.twig</strong> Expand source 
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence; collapse: true" data-theme="Confluence"><code>{% block multishop_email_image %}
    {% set siteaccess = basket is defined and basket.dataMap.siteaccess is defined ? basket.dataMap.siteaccess : null %}
    &lt;div&gt;
        &lt;img src=&quot;{{ absolute_url(asset(&#39;bundles/silversolutionseshop/img/email-header.png&#39;)) }}&quot;
             alt=&quot;{{ &#39;Please enable loading images to view this image&#39;|st_translate(null, {}, null, siteaccess) }}&quot;&gt;
    &lt;/div&gt;
{% endblock %}</code></pre>

</li>
</ul>
</td>
<td><p><a href="attachments/23560862/23561625.pdf">OwnerEmailOrder.pdf</a></p></td>
<td><div class="content-wrapper">
<strong>vendor/silversolutions/silver.e-shop/src/Siso/Bundle/LocalOrderManagementBundle/EventListener/OrderSplittingListener.php</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$shopOwnerMailReceiver = isset($emailAddresses[&#39;shopOwnerMailReceiver&#39;])
 ? $emailAddresses[&#39;shopOwnerMailReceiver&#39;] : &#39;&#39;;
$copiedBasket-&gt;setConfirmationEmail($shopOwnerMailReceiver);</code></pre>
<p><br />
</p>
<pre><code></code></pre>
<pre><code></code></pre>
</td>
</tr>
<tr>
<td>Sales Contact person</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p>This email is only send, when the email for the sales contact is configured.</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$recipientSalesContact = $basket-&gt;getSalesConfirmationEmail();
if (!empty($recipientSalesContact)) {
    $this-&gt;sendMailToRecipient($recipientSalesContact, $basket, true);
}</code></pre>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>#possible mode: config or customer
siso_checkout.default.order_confirmation.sales_email_mode: customer
siso_checkout.default.order_confirmation.sales_email_address:</code></pre>
<p>It is a copy of the email to the owner of the shop with an additional variable is_sales_contact (set to true).</p>
<p>The subject of this email can also differ:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>if (!empty($isSalesContact)){
    $subject = isset($this-&gt;mailSettings[&#39;sales_contact_subject&#39;])
        ? $this-&gt;mailSettings[&#39;sales_contact_subject&#39;]
        : $subject;
}</code></pre>
</td>
<td><div class="content-wrapper">
<p><br />
</p>
</td>
<td><div class="content-wrapper">
<p><br />
</p>
<p><br />
</p>
</td>
</tr>
</tbody>
</table>

``` 
 {% if line.catalogElement.sku == '150103' or line.catalogElement.sku == '150106' or line.catalogElement.sku == 'FVA10' %}
         <input type="hidden" name="ses_basket[{{ loop.index }}][MAIN]" value="1"/>
 {% endif %}
```

## Attachments:

![](images/icons/bullet_blue.gif) [Fwd\_ common.shop\_owner\_mail\_su.eml](attachments/23560862/23563654.eml) (application/x-upload-data)  
![](images/icons/bullet_blue.gif) [Fwd\_ Bestellbestätigung.eml](attachments/23560862/23563656.eml) (application/x-upload-data)  
![](images/icons/bullet_blue.gif) [FW\_ Bestellbestätigung Custome.eml](attachments/23560862/23563669.eml) (application/x-upload-data)  
![](images/icons/bullet_blue.gif) [buyer.pdf](attachments/23560862/23563657.pdf) (application/pdf)  
![](images/icons/bullet_blue.gif) [Fwd: common.shop\_owner\_mail\_subject.pdf](attachments/23560862/23563670.pdf) (application/pdf)  
![](images/icons/bullet_blue.gif) [order.pdf](attachments/23560862/23563671.pdf) (application/pdf)  
![](images/icons/bullet_blue.gif) [OwnerEmailOrder.pdf](attachments/23560862/23561623.pdf) (application/pdf)  
![](images/icons/bullet_blue.gif) [OwnerEmailOrder.pdf](attachments/23560862/23561625.pdf) (application/pdf)  
