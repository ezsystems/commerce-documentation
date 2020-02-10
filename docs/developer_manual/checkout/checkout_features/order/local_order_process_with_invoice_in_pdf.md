#  Local order process (with invoice in PDF) 

If the shop is not connected to an ERP system, eZ Commerce offers a possibility to still follow the order process and create a local order that will generate an invoice PDF and send it by mail.

The orders will be stored locally in the shop.

## Generate local orders - "SisoLocalOrderManagementBundle"

By default the shop sends the order after the checkout process to ERP. But there is also the possibility to disable the usage of ERP and use 'local order process' instead.

Disable ERP in order to use "Local Documents":

**LocalOrderManagementBundle/Resources/config/local\_order\_management.yml**

``` 
#set this to true in core and override this in the parameters.yml to false
siso_local_order_management.default.send_order_to_erp: true
siso_local_order_management.default.shop_owner_customer_number: 10000
```

For the implementation of this feature ***event listeners*** are used.

## Listeners: onRequestEvent and *onExceptionMessage*

In order to avoid sending the order to ERP (see configuration above), an exception is thrown, that will interrupt the default process (sending to ERP).

The order will not be send, if:

  - The message request is the "Order message".
  - The order sending is disabled in the configuration.
  - The order does not contain a specific customer number (in this case the customer number of the shop owner - this is connected with the order splitting function).

If the previous conditions are fulfilled, "LocalOrderRequiredException" exception is thrown and the order is not send to ERP.

Please note that due to an exception during the ERP communication, the message and the order occur in the respective logs as failed\!
**LocalOrderManagementBundle/Resources/config/services.xml**

``` 
<!-- Listener to listen on the request event and throw an exception in order to interuppt the process -->
<service id="siso_local_order_management.send_order_listener" class="%siso_local_order_management.send_order_listener.class%">
    <argument type="service" id="ezpublish.config.resolver"/>
    <tag name="kernel.event_listener"  event="silver_eshop.request_message" method="onRequestEvent" />
</service>
```

In order to return a valid response to the previous (checkout) process, an *onExceptionMessage* event listener is implemented, that reacts to the "LocalOrderRequiredException".

When the LocalOrderRequiredException is thrown the listener "***onExceptionMessage***" gets the exception and creates the valid Response Document filled with local data (orders).

In other words the shop reacts like an ERP system, confirms the order and returns an order id.

The response has the same structure as if it was returned from ERP, so no additional changes in the template are required.

At the same time it will also **generate and store the invoice data**, and **generate a PDF** with the invoice information. Then this information will be **send  by mail**.

**LocalOrderManagementBundle/Resources/config/services.xml**

``` 
<service id="siso_local_order_management.confirmation_listener" class="%siso_local_order_management.confirmation_listener.class%" lazy="true">
    <argument type="service" id="silver_basket.basket_repository" />
    <argument type="service" id="siso_tools.mailer_helper" />
    <argument type="service" id="ezpublish.config.resolver"/>
    <argument type="service" id="siso_local_order_management.invoice_service" />
    <argument type="service" id="doctrine.orm.entity_manager" />
        <call method="setMailSettings">
            <argument>$ses_swiftmailer;siso_core$</argument>
        </call>
    <tag name="kernel.event_listener"  event="silver_eshop.exception_message" method="onExceptionMessage" />
</service>
```

## Generate the invoice data (new entity)

To store the invoice in the database Doctrine is used. There is a new entity with the invoice information.

Steps to store the invoice data

### 1- New invoice entity Invoice

**LocalOrderManagementBundle/Entity/Invoice.php** Expand source 

``` 
<?php
/**
 * Product silver.e-shop
 *
 * A powerful e-commerce solution for B2B online shops / portals and complex
 * online applications that have access to ERP data, usually in real time.
 * http://www.silversolutions.de/eng/Products/silver.e-shop
 *
 * This file contains the class Basket
 *
 * @copyright Copyright (C) 2013 silver.solutions GmbH. All rights reserved.
 * @license see vendor/silversolutions/silver.e-shop/license_txt_ger.pdf
 * @version $Version$
 * @package silver.e-shop
 */
namespace Siso\Bundle\LocalOrderManagementBundle\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
 * Invoice
 *
 * @ORM\Table(name="ses_invoice")
 * @ORM\Entity(repositoryClass="Siso\Bundle\LocalOrderManagementBundle\Entity\InvoiceRepository")
 */
class Invoice
{
    /**
     * @var integer
     *
     * @ORM\Column(name="id", type="integer")
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="AUTO")
     *
     */
    private $invoiceId;

    /**
     * @var integer
     *
     * @ORM\Column(name="invoice_number", type="integer")
     */
    private $invoiceNumber;

    /**
     * @var string
     *
     * @ORM\Column(name="invoice_prefix", type="string", length=255, nullable=true)
     */
    private $invoicePrefix;

    /**
     * @var integer
     *
     * @ORM\Column(name="basket_id", type="integer")
     */
    private $basketId;

    /**
     * @var string
     *
     * @ORM\Column(name="shop_id", type="string", length=255, nullable=true)
     */
    private $shopId;
    /**
     * Get invoiceId
     *
     * @return integer
     */
    public function getInvoiceId()
    {
        return $this->invoiceId;
    }

    /**
     * Set invoiceNumber
     *
     * @param integer $invoiceNumber
     *
     * @return Invoice
     */
    public function setInvoiceNumber($invoiceNumber)
    {
        $this->invoiceNumber = $invoiceNumber;

        return $this;
    }

    /**
     * Get invoiceNumber
     *
     * @return integer
     */
    public function getInvoiceNumber()
    {
        return $this->invoiceNumber;
    }

    /**
     * Set invoicePrefix
     *
     * @param string $invoicePrefix
     *
     * @return Invoice
     */
    public function setInvoicePrefix($invoicePrefix)
    {
        $this->invoicePrefix = $invoicePrefix;

        return $this;
    }

    /**
     * Get invoicePrefix
     *
     * @return string
     */
    public function getInvoicePrefix()
    {
        return $this->invoicePrefix;
    }

    /**
     * Set basketId
     *
     * @param integer $basketId
     *
     * @return Invoice
     */
    public function setBasketId($basketId)
    {
        $this->basketId = $basketId;

        return $this;
    }

    /**
     * Get basketId
     *
     * @return integer
     */
    public function getBasketId()
    {
        return $this->basketId;
    }

    /**
     * Set shopId
     *
     * @param string $shopId
     *
     * @return Invoice
     */
    public function setShopId($shopId)
    {
        $this->shopId = $shopId;

        return $this;
    }

    /**
     * Get shopId
     *
     * @return string
     */
    public function getShopId()
    {
        return $this->shopId;
    }
}
```

### 2- New repository required to work with the entity invoice*.*

**LocalOrderManagementBundle/Resources/config/services.xml**

``` 
<!-- repository service, it is an implementation of the invoice_repository -->
<service id="siso_local_order_management.invoice_repository" class="%siso_local_order_management.invoice_repository.class%">
    <factory service="doctrine.orm.entity_manager" method="getRepository"/>
    <argument type="string">SisoLocalOrderManagementBundle:Invoice</argument>
</service>
```

To generate the invoice entity it will use the following data:

  - **Invoice number**: if there is already any entry in the database it takes the last invoice number and increase it by one, otherwise, if the database is empty it gets the invoice number from the configuration.
  - **Invoice prefix**: It gets it from the configuration by siteaccess (see code below)
  - **Basket id**: It gets the current basket\_id
  - **Shop id**: shop  id, it gets the value from the configuration (important in multishop context)  

**LocalOrderManagementBundle/Resources/config/local\_order\_management.yml**

``` 
# Generate the invoice number for the local orders in multishops with no ERP
siso_local_order_management.default.invoice_number:
    start: 10000
    prefix: 'RE'
```

**EshopBundle/Resources/config/silver.eshop.yml**

``` 
#ShopID that is stored in the database
siso_core.default.shop_id: 'MAIN'
```

## Generate invoice PDF and send it by mail

At the end of the process the invoice with all the required information is created, stored in the DB,  this invoice is generated in PDF format, and it will be send also as attachment by email.

To generate the PDF a tool called "wkhtmltopdf" is used, so this tool has to be installed on the server. For more information check the site <http://wkhtmltopdf.org> and use the last stable version ("0.12.4").

To use the wkhtmltopdf is very easy, it only needs an URL with a valid HTML and the path where the PDF is going to be generated.

``` 
Use: wkhtmltopdf [URL] [Path.pdf]
Eg:  wkhtmltopdf http://harmony-dev.silver-eshop test.pdf
```

**  
**

**Path of wkhtmltopdf configuration  
**In some systems the path, where the tool is installed, can be different. So there is a new parameter to store the path of the "wkhtmltopdf".

**LocalOrderManagementBundle/Resources/config/local\_order\_management.yml**

``` 
    siso_local_order_management.default.wkhtmltopdf_server_path: '/usr/bin/wkhtmltopdf'
```

**Config to enable/disable generation of footer and/or header on each page for invoice PDF**

Usually the invoice PDF contains one header at the beginning and one footer at the end of the document.

**LocalOrderManagementBundle/Resources/config/local\_order\_management.yml**

``` 
    #Enable or disable the possibility to generate footer and header which are displayed on all PDF pages
    siso_local_order_management.default.generate_footer_for_pdf: false
    siso_local_order_management.default.generate_header_for_pdf: false
```

With enabling (set to true) this configuration there will be a header/footer for each (printed) page of the PDF. In this case the header and/or footer will be generated using these separate templates:

  - src/Silversolutions/Bundle/EshopBundle/Resources/views/Invoice/header.html.twig
  - src/Silversolutions/Bundle/EshopBundle/Resources/views/Invoice/footer.html.twig

These templates can be used directly or as a base to override them in the project. 

**Process to create the PDF**

The PDF content (and the header and/or footer) will be stored as HTML and directly removed after usage.

**vendor/silversolutions/silver.e-shop/src/Siso/Bundle/LocalOrderManagementBundle/Service/InvoiceService.php** Expand source 

``` 
public function generateInvoicePdf($invoiceNumber)
{
...
//Create the path to store the generated pdf
$invoiceTmpPath = $this->configResolver->getParameter(
    'invoice_pdf_tmp_path',
    'siso_local_order_management'
);
$invoiceParameters = $this->configResolver->getParameter(
    'invoice_number',
    'siso_local_order_management'
);
//In some environments is required to the full path of the tool install in the server
$wkhtmltopdfPath = $this->configResolver->getParameter(
    'wkhtmltopdf_server_path',
    'siso_local_order_management'
);

$invoicePrefix = isset($invoiceParameters['prefix']) ? $invoiceParameters['prefix'] : '';
$pdfName = $this->transService->translate('common.invoice_') . $invoicePrefix . $invoiceNumber . '.pdf';
$pdfPath =  $invoiceTmpPath . $pdfName;
$footerCommandPart = '';
$headerCommandPart = '';

if ($generateFooter && !empty($footerPathHtmlFile)) {
    $footerCommandPart = ' --footer-html '.$footerPathHtmlFile;
}
if ($generateHeader && !empty($headerPathHtmlFile)) {
    $headerCommandPart = ' --header-html '.$headerPathHtmlFile;
}

$command = $wkhtmltopdfPath.$footerCommandPart.$headerCommandPart.' '.$invoicePathHtmlFile.' '.$pdfPath;

//Used Process component to execute the command line
/** @var Process $process */
$process = new Process(sprintf($command));
$process->run();
...
}
```

The first part of name of the PDF is translatable ('common.invoice\_') and the second part consist of a prefix and the invoice number.

Example of the PDF generated [invoice\_RE10000.pdf](attachments/23560832/23563506.pdf)

Once the PDF is generated and the order is stored in the database. It will attach this invoice PDF to the email that is send after the order confirmation.

**  
**

**Send emails with attachments.**

The PDF will be attach to the email that generated the order process.

And additional to the standard email process it will send also another email the the owner of the shop. This shop owner is defined as a parameter in the configuration (It is defined by site-access).

``` 
siso_core.default.ses_swiftmailer:
    mailSender: noreply@silversolutions.de
    mailReceiver: noreply@silversolutions.de
    ...
    shopOwnerMailReceiver: noreply@silversolutions.de
```

**Emails**

All these emails are based on the standard order confirmation email templates: 

'SilversolutionsEshopBundle:Checkout/Email:order\_confirmation.txt.twig' and 'SilversolutionsEshopBundle:Checkout/Email:order\_confirmation.html.twig'

<table>
<thead>
<tr class="header">
<th>Receiver</th>
<th>Info</th>
<th>PDF</th>
<th>Status</th>
<th>Code</th>
</tr>
</thead>
<tbody>
<tr>
<td>Buyer</td>
<td>Email with invoice attached</td>
<td><div class="content-wrapper">
<p><a href="/download/attachments/23560862/FW_%20Bestellbest%C3%A4tigung%20Custome.eml?version=1&amp;modificationDate=1492682267000&amp;api=v2" class="confluence-embedded-file"><img src="download/resources/com.atlassian.confluence.plugins.confluence-view-file-macro:view-file-macro-resources/images/placeholder-medium-file.png" height="250" /><span class="title">FW_ Bestellbestätigung Custome.eml</a></p>
</td>
<td><br />
</td>
<td><div class="content-wrapper">
<pre><code></code></pre>
<pre><code></code></pre>
<strong>vendor/silversolutions/silver.e-shop/src/Siso/Bundle/LocalOrderManagementBundle/EventListener/OrderConfirmationListener.php</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$recipientBuyer = $basket-&gt;getConfirmationEmail();
...
if (!empty($recipientBuyer)) {
 $this-&gt;sendMailToRecipient($recipientBuyer, $basket, $invoice, $invoicePdfPath);
}</code></pre>
</td>
</tr>
<tr>
<td>Sales Contact person</td>
<td><div class="content-wrapper">
<p>Copy of the buyer email with only a few differences.</p>
<p>Therefor the variable is_sales_contact is set to true.</p>
<ul>
<li><p>Send only  if configured</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>#possible mode: config or customer
siso_checkout.default.order_confirmation.sales_email_mode: customer
siso_checkout.default.order_confirmation.sales_email_address:</code></pre>

</li>
</ul>
</td>
<td><br />
</td>
<td><br />
</td>
<td><div class="content-wrapper">
<strong>vendor/silversolutions/silver.e-shop/src/Siso/Bundle/LocalOrderManagementBundle/EventListener/OrderConfirmationListener.php</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$recipientSalesContact = $basket-&gt;getSalesConfirmationEmail();
...
if (!empty($recipientSalesContact)) {
    $isShopOwner = false;
    $isSalesContact = true;
    $this-&gt;sendMailToRecipient($recipientSalesContact, $basket, $invoice, $invoicePdfPath, $isShopOwner, $isSalesContact);
}</code></pre>
</td>
</tr>
<tr>
<td>Owner of the shop</td>
<td><p>This email is also a copy of the email that is send to the buyer.</p>
<p>And special message and subject are set for it</p>
<p>"shop_owner_mail_subject" (Set in the email configuration)</p>
<p><span class="external">'email_shop_owner_intro_text' (TextModule inside the BackEnd)</p>
<p>Therefor the variable is_shop_owner is set to true.</p></td>
<td><div class="content-wrapper">
<p><a href="/download/attachments/23560862/Fwd_%20common.shop_owner_mail_su.eml?version=1&amp;modificationDate=1492682267000&amp;api=v2" class="confluence-embedded-file"><img src="download/resources/com.atlassian.confluence.plugins.confluence-view-file-macro:view-file-macro-resources/images/placeholder-medium-file.png" height="250" /><span class="title">Fwd_ common.shop_owner_mail_su.eml</a></p>
</td>
<td><br />
</td>
<td><div class="content-wrapper">
<pre><code></code></pre>
<pre><code></code></pre>
<strong>vendor/silversolutions/silver.e-shop/src/Siso/Bundle/LocalOrderManagementBundle/EventListener/OrderConfirmationListener.php</strong>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>$emailAddresses = $this-&gt;configResolver
 -&gt;getParameter(&#39;ses_swiftmailer&#39;, &#39;siso_core&#39;);

$shopOwnerMailReceiver = isset($emailAddresses[&#39;shopOwnerMailReceiver&#39;])
 ? $emailAddresses[&#39;shopOwnerMailReceiver&#39;] : null;
...
if (!empty($shopOwnerMailReceiver)) {
 $isShopOwner = true;
 $this-&gt;sendMailToRecipient($shopOwnerMailReceiver, $basket, $invoice, $invoicePdfPath, $isShopOwner);
}</code></pre>
</td>
</tr>
</tbody>
</table>

See [OrderHistory - Local Orders when ERP not available](OrderHistory---Local-Orders-when-ERP-not-available_23560888.html)

## Attachments:

![](images/icons/bullet_blue.gif) [invoice\_RE10000.pdf](attachments/23560832/23563506.pdf) (application/pdf)  
