#  How does the shop communicate with the ERP 

## General 

The shop is able to use different transports in order to connect to the ERP systems or other third parties. eZ Commerce (Advanced version only) supports different transport protocols such as

  - Webservices/SOAP
  - REST

The transport layer can be extended in project if there is a need for an additional protocol or way to access the ERP/CRM...

One way to connect to an SAP or Microsoft Dynamics NAV system is to use the Web.Connector (separately licensed product) which already comes with prepared interfaces to the stated ERP systems.  There are other options besides using the Web.Connector:

  - if the ERP offers a Webservice/REST interface it would be an option to connect eZ Commerce Advanced directly with the ERP
  - An enterprise service bus (ESB) would be an option as well

The following picture describes how the shop communicates with the ERP system using the Web.Connector: 

![](attachments/23560630/23569218.png)

Since every ERP system uses its own standard how data is formatted and named eZ Commerce Advanced uses a standard format to describe data such as customer or orders. The standard used is **UBL** ([Universal Business Language](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=ubl)) an universal standard describing business processes. 

## What happens when a request is sent to an ERP-System

| Step | Who                        | What                                                                                               |
| ---- | -------------------------- | -------------------------------------------------------------------------------------------------- |
| 1    | Shop          | Requests data using a request object                                                               |
| 2    | Shop          | A mapping might map the UBL request to a specific request for the ERP (optional step) |
| 3    | Web.Connector              | The Web.Connector receives the request and maps the request into an ERP request                    |
| 4    | Web.Connector | Sends the ERP request to the ERP                                                                   |
| 5    | ERP                        | The ERP processes the requests and sends an answer                                                 |
| 6    | Web.Connector | Gets answer from ERP and maps it to an answer for the shop                                         |
| 7    | Shop                       | Receives the request and maps it to UBL (optional step)                                            |
| 8    | Shop                       | Application gets a proper UBL response object                                                      |

Specification for requests and repsonses

The shop uses a specification which describes how the request to the ERP (our Web.Connector) and the answer from the ERP will look like. 

The specification is written in XML and located in resources/specifications/xml. It includes always two files: a request and a response specification. 

It it used to [generate PHP objects](ERP-Message-Class-Generator_23560376.html) which can be used to define a request or get the response back from the ERP.

An example - get customer data

**The request is initiated by the shop:**

<table>
<thead>
<tr class="header">
<th>Step</th>
<th>Who</th>
<th>Details</th>
<th>Mapping</th>
</tr>
</thead>
<tbody>
<tr>
<td>UBL-Request</td>
<td><strong>Shop</strong></td>
<td><p>Sets request using the generated ERP-Classes for select customer.</p>
<p>Customer now will be set in</p>
<p>BuyerCustomerParty/Party/PartyIdentification/ID</p></td>
<td><div class="content-wrapper">
<p>Used specification:</p>
<p>@EshopBundle/Resources/specifications/xml/request.selectCustomer.xml</p>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;BuyerCustomerParty&gt;
    &lt;Party ses_unbounded=&quot;PartyIdentification&quot;&gt;
        &lt;PartyIdentification&gt;
            &lt;ID&gt;10000&lt;/ID&gt;
        &lt;/PartyIdentification&gt;
    &lt;/Party&gt;
&lt;/BuyerCustomerParty&gt;</code></pre>
</td>
</tr>
<tr>
<td>maps request</td>
<td> <strong>Shop</strong></td>
<td>Request is mapped to<br />

<pre><code>&lt;PARTY&gt;&lt;PARTY_ID&gt;0100000&lt;/PARTY_ID&gt;&lt;/PARTY&gt;</code></pre></td>
<td><div class="content-wrapper">
<p>Used mapping</p>
<p>app/Resources/xsl/request.selectcustomer.xsl</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;xsl:stylesheet version=&quot;1.0&quot; xmlns:xsl=&quot;http://www.w3.org/1999/XSL/Transform&quot;&gt;
    &lt;xsl:template match=&quot;/BuyerCustomerParty&quot;&gt;
             &lt;PARTY&gt;
                 &lt;PARTY_ID&gt;
                     &lt;xsl:value-of select=&quot;Party/PartyIdentification/ID&quot; /&gt;
                 &lt;/PARTY_ID&gt;
             &lt;/PARTY&gt;
    &lt;/xsl:template&gt;
&lt;/xsl:stylesheet&gt;</code></pre>
</td>
</tr>
<tr>
<td>Receives request</td>
<td><strong>Web.Connector</strong> </td>
<td>Converts the request in the language of the ERP, e.g.<br />

<pre><code>&lt;Params&gt;&lt;Read&gt;&lt;No&gt;0100000&lt;/No&gt;&lt;/Read&gt;&lt;/Params&gt;</code></pre></td>
<td><div class="content-wrapper">
 Used mapping<br />

<pre><code>mapping/nav_ws/xsl/request.selectcustomer.xsl</code></pre>
<div>
<p><br />
</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;xsl:stylesheet version=&quot;1.0&quot;
    xmlns:xsl=&quot;http://www.w3.org/1999/XSL/Transform&quot;&gt;
    &lt;xsl:param name=&quot;WEBSITE&quot; /&gt;
    &lt;xsl:template match=&quot;/&quot;&gt;
        &lt;Params&gt;
            &lt;Read&gt;
                &lt;No&gt;&lt;xsl:value-of select=&quot;PARTY/PARTY_ID&quot; /&gt;&lt;/No&gt;
            &lt;/Read&gt;
        &lt;/Params&gt;
    &lt;/xsl:template&gt;
&lt;/xsl:stylesheet&gt;
</code></pre>

</td>
</tr>
<tr>
<td> Processes request</td>
<td> <strong>ERP-System</strong></td>
<td> 
<p>Gets the request and will send a</p>
<p>result back</p></td>
<td><br />
</td>
</tr>
</tbody>
</table>

**The answer will be mapped one or more time depending on the configuration:**

**  
**

**  
**

<table>
<thead>
<tr class="header">
<th>Step</th>
<th>Who</th>
<th>Details</th>
<th>Mapping</th>
</tr>
</thead>
<tbody>
<tr>
<td>receives customer data from ERP</td>
<td><strong>Web.Connector</strong></td>
<td><div class="content-wrapper">
<p>Data from ERP</p>
<strong></strong> Expand source 
<pre class="" data-syntaxhighlighter-params="brush: html/xml; gutter: false; theme: Confluence; collapse: true" data-theme="Confluence"><code>&lt;RawResponse&gt;
 &lt;Key&gt;28;EgA...&lt;/Key&gt;
  &lt;No&gt;0100000&lt;/No&gt;
  &lt;Blocked&gt;_blank_&lt;/Blocked&gt;
  &lt;Name&gt;Demo customer&lt;/Name&gt;
  &lt;Name_2&gt;- Office  -&lt;/Name_2&gt;
  &lt;Address&gt;Demo street&lt;/Address&gt;
  &lt;Post_Code&gt;12555&lt;/Post_Code&gt;
  &lt;City&gt;Berlin&lt;/City&gt;
  &lt;Country_Region_Code&gt;DE&lt;/Country_Region_Code&gt;
  &lt;Fax_No&gt;0301212&lt;/Fax_No&gt;
  &lt;Phone_No&gt;03012121&lt;/Phone_No&gt;
  &lt;E_Mail&gt;info@test.de&lt;/E_Mail&gt;
  &lt;Contact&gt;- -&lt;/Contact&gt;
  &lt;Customer_Posting_Group&gt;SAMMEL&lt;/Customer_Posting_Group&gt;
  &lt;Gen_Bus_Posting_Group&gt;NATIONAL&lt;/Gen_Bus_Posting_Group&gt;
  &lt;VAT_Bus_Posting_Group&gt;NATIONAL&lt;/VAT_Bus_Posting_Group&gt;
   
  &lt;Kontakt_EMail&gt;info@test.de&lt;/Kontakt_EMail&gt;
  &lt;Kontakt_Ansprechpartner&gt;- -&lt;/Kontakt_Ansprechpartner&gt;
  &lt;PaymentMethodCode&gt;EINZUG&lt;/PaymentMethodCode&gt;
  &lt;Ship_to_Address&gt;
    &lt;Cust_Ship_to_Address_WS&gt;
      &lt;Key&gt;44;3gAAAAJ7BzAAMQAwADAAMAAwADAAAAACe/9MADAAMQ==9;1326112700;&lt;/Key&gt;
      &lt;Customer_No&gt;0100000&lt;/Customer_No&gt;
      &lt;Code&gt;L01&lt;/Code&gt;
      &lt;Name&gt;Demo customer&lt;/Name&gt;
      &lt;Name_2&gt;Max Mueller&lt;/Name_2&gt;
      &lt;Address&gt;Molkereistr. 1a&lt;/Address&gt;
      &lt;City&gt;Berlin&lt;/City&gt;
      &lt;Post_Code&gt;12555&lt;/Post_Code&gt;
      &lt;Blocked/&gt;
    &lt;/Cust_Ship_to_Address_WS&gt;
    
  &lt;/Ship_to_Address&gt;
&lt;/RawResponse&gt;</code></pre>
</td>
<td><p><br />
</p>
<p><br />
</p></td>
</tr>
<tr>
<td>Maps data back</td>
<td>Web.Connector</td>
<td>Maps data for shop using a 1:1 vonversion</td>
<td><div class="content-wrapper">
<p>Using a mapping file</p>
<pre><code>mapping/nav_ws/xsl/response.selectcustomer.xsl</code></pre>
<pre><code>In this case the answer will not be mapped and mapping is done </code></pre>
<p>in the shop</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;xsl:stylesheet version=&quot;1.0&quot;
    xmlns:xsl=&quot;http://www.w3.org/1999/XSL/Transform&quot;&gt;
    &lt;xsl:param name=&quot;WEBSITE&quot; /&gt;
    &lt;xsl:template match=&quot;/SoapResponse/ReadResult&quot;&gt;
        &lt;RawResponse&gt;
            &lt;xsl:copy-of select=&quot;*&quot; /&gt;
        &lt;/RawResponse&gt;
    &lt;/xsl:template&gt;
&lt;/xsl:stylesheet&gt;</code></pre>
</td>
</tr>
<tr>
<td>Receivers repsonse and maps it to UBL</td>
<td>Shop</td>
<td>Converts the answer into UBL using a mapping and the shop will convert the response to an UBL object as defined in the specification (@EshopBundle/Resources/specifications/xml/response.selectCustomer.xml)</td>
<td><div class="content-wrapper">
<p>Converts the answer into UBL using a mapping and the shop will convert the response to an UBL object as defined in the specification (@EshopBundle/Resources/specifications/xml/response.selectCustomer.xml)</p>
<br />

<p>Mapping (app/Resources/xsl/response.selectcustomer.xsl):</p>
<p><br />
</p>
<strong></strong> Expand source 
<pre class="" data-syntaxhighlighter-params="brush: html/xml; gutter: false; theme: Confluence; collapse: true" data-theme="Confluence"><code>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;xsl:stylesheet version=&quot;1.0&quot; xmlns:xsl=&quot;http://www.w3.org/1999/XSL/Transform&quot;&gt;
    &lt;xsl:template match=&quot;/RawResponse&quot;&gt;
        &lt;OrderResponse singleElementArrays=&quot;OrderLine&quot;&gt;
                &lt;BuyerCustomerParty&gt;
                    &lt;Party&gt;
                        &lt;PartyIdentification&gt;
                            &lt;ID&gt;
                                &lt;xsl:value-of select=&quot;No&quot; /&gt;
                            &lt;/ID&gt;
                        &lt;/PartyIdentification&gt;
                        &lt;PartyName&gt;
                            &lt;Name&gt;
                                &lt;xsl:value-of select=&quot;Name&quot; /&gt;
                            &lt;/Name&gt;
                        &lt;/PartyName&gt;
                        &lt;PartyName&gt;
                            &lt;Name&gt;
                                &lt;xsl:value-of select=&quot;Name_2&quot; /&gt;
                            &lt;/Name&gt;
                        &lt;/PartyName&gt;
                        &lt;PostalAddress&gt;
                            &lt;StreetName&gt; &lt;xsl:value-of select=&quot;Address&quot; /&gt;&lt;/StreetName&gt;
                            &lt;BuildingName&gt;&lt;/BuildingName&gt;
                            &lt;BuildingNumber&gt;&lt;/BuildingNumber&gt;
                            &lt;CityName&gt;&lt;xsl:value-of select=&quot;City&quot; /&gt;&lt;/CityName&gt;
                            &lt;PostalZone&gt;&lt;xsl:value-of select=&quot;Post_Code&quot; /&gt;&lt;/PostalZone&gt;
                            &lt;CountrySubentity&gt;&lt;/CountrySubentity&gt;
                            &lt;AddressLine&gt;

                            &lt;/AddressLine&gt;
                            &lt;Country&gt;
                                &lt;IdentificationCode&gt;DE&lt;/IdentificationCode&gt;
                            &lt;/Country&gt;
                        &lt;/PostalAddress&gt;
                        &lt;Contact&gt;
                            &lt;Name&gt;&lt;xsl:value-of select=&quot;Kontakt_Ansprechpartner&quot; /&gt;&lt;/Name&gt;
                            &lt;Telephone&gt;&lt;/Telephone&gt;
                            &lt;Telefax&gt;&lt;/Telefax&gt;
                            &lt;ElectronicMail&gt;&lt;xsl:value-of select=&quot;Kontakt_EMail&quot; /&gt;&lt;/ElectronicMail&gt;
                        &lt;/Contact&gt;
                        &lt;SesExtension&gt;
                            &lt;CustomerPostingGroup&gt;
                                &lt;xsl:value-of select=&quot;Customer_Posting_Group&quot; /&gt;
                            &lt;/CustomerPostingGroup&gt;
                            &lt;Gliederungskennzeichen&gt;
                                &lt;xsl:value-of select=&quot;Gliederungskennzeichen&quot; /&gt;
                            &lt;/Gliederungskennzeichen&gt;
                        &lt;/SesExtension&gt;
                    &lt;/Party&gt;
                &lt;/BuyerCustomerParty&gt;
                &lt;SellerSupplierParty&gt;
                &lt;/SellerSupplierParty&gt;
            &lt;AccountingCustomerParty&gt;
                &lt;Party singleElementArrays=&quot;PartyIdentification PartyName&quot;&gt;
                    &lt;PartyIdentification&gt;
                        &lt;ID&gt;&lt;xsl:value-of select=&quot;CUSTOMER/NUMBER&quot; /&gt;&lt;/ID&gt;
                    &lt;/PartyIdentification&gt;
                    &lt;PartyName&gt;
                        &lt;Name&gt;
                            &lt;xsl:value-of select=&quot;CUSTOMER/NAME&quot; /&gt;
                        &lt;/Name&gt;
                    &lt;/PartyName&gt;
                    &lt;PostalAddress singleElementArrays=&quot;AddressLine&quot;&gt;
                    &lt;/PostalAddress&gt;
                &lt;/Party&gt;
            &lt;/AccountingCustomerParty&gt;
            &lt;OriginatorCustomerParty&gt;
            &lt;/OriginatorCustomerParty&gt;
            &lt;DeliveryParties singleElementArrays=&quot;Party&quot;&gt;
                &lt;xsl:variable name=&quot;CustomerNumber&quot; select=&quot;CUSTOMER/NUMBER&quot;&gt;&lt;/xsl:variable&gt;
                &lt;xsl:variable name=&quot;CustomerName&quot; select=&quot;CUSTOMER/NAME&quot;&gt;&lt;/xsl:variable&gt;
                &lt;xsl:for-each select=&quot;CUSTOMER/SHIP_TO_ADDRESS&quot;&gt;
                    &lt;Party singleElementArrays=&quot;PartyIdentification PartyName&quot;&gt;
                        &lt;PartyIdentification&gt;
                            &lt;ID&gt;
                                &lt;xsl:value-of select=&quot;SHIP_CODE&quot; /&gt;
                            &lt;/ID&gt;
                        &lt;/PartyIdentification&gt;
                        &lt;PartyName&gt;
                            &lt;Name&gt;
                                &lt;xsl:value-of select=&quot;$CustomerName&quot; /&gt;
                            &lt;/Name&gt;
                        &lt;/PartyName&gt;
                        &lt;PostalAddress singleElementArrays=&quot;AddressLine&quot;&gt;
                        &lt;/PostalAddress&gt;
                    &lt;/Party&gt;
                &lt;/xsl:for-each&gt;
            &lt;/DeliveryParties&gt;
            &lt;SesExtension&gt;
                &lt;CustomerPostingGroup&gt;
                    &lt;xsl:value-of select=&quot;CUSTOMER/CUSTOMER_POSTING_GROUP&quot; /&gt;
                &lt;/CustomerPostingGroup&gt;
            &lt;/SesExtension&gt;
            &lt;OrderLine&gt;
            &lt;/OrderLine&gt;
        &lt;/OrderResponse&gt;
    &lt;/xsl:template&gt;
&lt;/xsl:stylesheet&gt;</code></pre>
<p><br />
</p>
<p>SPecification</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;OrderResponse singleElementArrays=&quot;OrderLine&quot;&gt;
  &lt;BuyerCustomerParty&gt;
    &lt;Party&gt;
      &lt;PartyIdentification&gt;
        &lt;ID&gt;0838020&lt;/ID&gt;
      &lt;/PartyIdentification&gt;
      &lt;PartyName&gt;
        &lt;Name&gt;Demo customer&lt;/Name&gt;
      &lt;/PartyName&gt;
      &lt;PartyName&gt;
        &lt;Name&gt;Max Meier&lt;/Name&gt;
      &lt;/PartyName&gt;
      &lt;PostalAddress&gt;
        &lt;StreetName&gt;Demo street 12&lt;/StreetName&gt;
        &lt;BuildingName/&gt;
        &lt;BuildingNumber/&gt;
        &lt;CityName&gt;Berlin/CityName&gt;
        &lt;PostalZone&gt;12555&lt;/PostalZone&gt;
        &lt;CountrySubentity/&gt;
        &lt;AddressLine/&gt;
        &lt;Country&gt;
          &lt;IdentificationCode&gt;DE&lt;/IdentificationCode&gt;
        &lt;/Country&gt;
      &lt;/PostalAddress&gt;
      &lt;Contact&gt;
        &lt;Name&gt;- -&lt;/Name&gt;
        &lt;Telephone/&gt;
        &lt;Telefax/&gt;
        &lt;ElectronicMail&gt;info@test.de&lt;/ElectronicMail&gt;
      &lt;/Contact&gt;
      &lt;SesExtension&gt;
        &lt;CustomerPostingGroup&gt;SAMMEL&lt;/CustomerPostingGroup&gt;
       
      &lt;/SesExtension&gt;
    &lt;/Party&gt;
  &lt;/BuyerCustomerParty&gt;
  &lt;SellerSupplierParty/&gt;
  &lt;AccountingCustomerParty&gt;
    &lt;Party singleElementArrays=&quot;PartyIdentification PartyName&quot;&gt;
      &lt;PartyIdentification&gt;
        &lt;ID/&gt;
      &lt;/PartyIdentification&gt;
      &lt;PartyName&gt;
        &lt;Name/&gt;
      &lt;/PartyName&gt;
      &lt;PostalAddress singleElementArrays=&quot;AddressLine&quot;/&gt;
    &lt;/Party&gt;
  &lt;/AccountingCustomerParty&gt;
  &lt;OriginatorCustomerParty/&gt;
  &lt;DeliveryParties singleElementArrays=&quot;Party&quot;/&gt;
  &lt;SesExtension&gt;
    &lt;CustomerPostingGroup/&gt;
  &lt;/SesExtension&gt;
  &lt;OrderLine/&gt;
&lt;/OrderResponse&gt;</code></pre>
</td>
</tr>
</tbody>
</table>

**  
**

## Web.Connector setup - a short introduction

In eZ Commerce Advanced some config parameters have to be setup:

The URL to the Web.Connector:

webconnector\_url: [https://\<ip: port\>/webconnector/webconnector\_opentrans.php5](https://88.79.227.54:2443/webconnector_dlrg/webconnector_opentrans.php5)

and the configuration for each message:

``` 
silver_erp.config.messages:
       .........
        select_customer:
            message_class: "Silversolutions\\Bundle\\EshopBundle\\Message\\SelectCustomerMessage"
            response_document_class: "\\Silversolutions\\Bundle\\EshopBundle\\Entities\\Messages\\Document\\CustomerResponse"
            webservice_operation: "SV_OPENTRANS_SELECT_CUSTOMERINFO"
            mapping_identifier: "selectcustomer"
```

If a mapping\_identifier is defined eZ Commerce will use a mapping (XSLT based). 

On the Web.Connector PC for each message a configuration describes which mapping shall be performed: 

``` 
     $cfg->setSetting('WebserviceHandler_Config', 'MethodSettings', array(
        ........
        'SV_OPENTRANS_SELECT_CUSTOMERINFO' => array(
            'remote_reference'      => 'customer',
            'method'                => 'Read',
            'mapping'               => 'selectCustomer',
            'mapping_parameters'    => array(
                'WEBSITE' => 'HOME',
            ),
        ),
```

## Example for developers

``` 
     $erpService = $this->getContainer()->get('silver_erp.facade');
     
     $customerNo = "1000";
     $selectCustomerResponse = $erpService->selectCustomer($customerNo);
```

The answer will be an object as defined in @EshopBundle/Resources/specifications/xml/response.selectCustomer.xml:

``` 
Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\CustomerResponse Object
(
    [BuyerCustomerParty] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\CustomerResponseBuyerCustomerParty Object
        (
            [Party] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\Party Object
                (
                    [PartyIdentification] => Array
                        (
                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PartyPartyIdentification Object
                                (
                                    [ID] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                        (
                                            [value] => 0100000
                                        )
                                )
                        )
                    [PartyName] => Array
                        (
                            [0] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PartyPartyName Object
                                (
                                    [Name] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                        (
                                            [value] => Demo customer
                                        )
                                )
                            [1] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PartyPartyName Object
                                (
                                    [Name] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                        (
                                            [value] => - Office -
                                        )
                                )
                        )
                    [PostalAddress] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\PartyPostalAddress Object
                        (
                            [StreetName] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => Demo street 12
                                )
                            [AdditionalStreetName] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] =>
                                )
                            [BuildingNumber] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] =>
                                )
                            [CityName] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => Berlin
                                )
                            [PostalZone] => Silversolutions\Bundle\EshopBundle\Entities\Messages\Document\StringObject Object
                                (
                                    [value] => 12555
                                )
....
```

## Attachments:

![](images/icons/bullet_blue.gif) [silver.e-shop-ERP-Integration\_content\_whole\_width.png](attachments/23560630/23563400.png) (image/png)  
![](images/icons/bullet_blue.gif) [silver.e-shop-ERP-Integration.png](attachments/23560630/23563378.png) (image/png)  
![](images/icons/bullet_blue.gif) [web.connector\_schaubild\_alle\_ERPs.png](attachments/23560630/23569218.png) (image/png)  
