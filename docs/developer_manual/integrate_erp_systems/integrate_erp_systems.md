#  Integrate ERP systems 

eZ Commerce is build to communicate with ERP systems. It uses the data and logic already provided by the ERP system. 

This ensures that the

  - shop is always up to date
  - the complex logic and processes already implemented in the ERP will be reused 
  - the customer will see his individual prices and the availability from the ERP
  - seemless processes: orders will be transferred 

## How to start

The introduction [How does the shop communicate with the ERP](How-does-the-shop-communicate-with-the-ERP_23560630.html) will explain how the shop communicates with the ERP system. It also describes how to adapt the communication for your needs.

### How to configure the ERP integration

If you are using a prepared connector (Connector) you will find further information about the configuration here: [Web-Connector Configuration](Web-Connector-Configuration_23560325.html)

In case a REST based ERP system is used see: [Curl Configuration](Curl-Configuration_23560254.html)

In case your ERP supports Web-Services directly: [Configuration for Webservice based ERPs](Configuration-for--Webservice-based-ERPs_23561087.html)

### How to adapt the mapping

eZ Commerce is using a standard "language" (UBL) to model the business data. The ERP systems normally are using vendor specific structures and attribute names. A xslt based mapping feature allows to adapt the mapping between these formats. See [Adapt the mappings for ERP functions](Adapt-the-mappings-for-ERP-functions_23561088.html) for more details. 

### Howto monitor the ERP connection

The Admin user interface provides a monitoring service which allows to check all messages exchanged between ERP and eZ Commerce. 

After selecting a date range and a measuring point (recommended ones:  "Request data before and after mapping" and "Response data before and after mapping" ) you will get more details about the request send to the ERP system and the mapping which has applied. 

  - mapping:   displays the xslt file used for mapping
  - the input XML and the XML converted by the xslt transformation  

**Tip**:

You can copy the input xml and copy the content to PHP-storm. This allows to test and adapt the mapping using the stated xslt file without testing it using the shop.

![](attachments/23560631/23566389.png)

### How to use the ERP api for special requirements

Besides the standard processes eZ Commerce offers an API to call ERP functions in your application. Check [ERP communication](ERP-communication_23560973.html) for more details.

## Supported ERP systems

eZ Commerce offers prepared interfaces to 

  - Microsoft Dynamics NAV
  - Microsoft Dynamics AX
  - SAP

For the listed ERP systems connectors are available.

Since eZ Commerce offers an open interface using standards (REST, Webservices and a standard XML Format UBL) it is possible to adapt other ERP systems as well.

Requirements towards an ERP system: The ERP system has to be open (means offers e.g. a REST or Web-Service interface) and offers the functions as listed in "Supported processes with ERP integration" .

## Supported processes with ERP integration

This chapter will describe which data is exchanged between eZ Commerce Advanced and the ERP system. It depends on the project specific configuration which data will be exchanged since in complex systems other systems might provide data as well (such as a PIM system).

<table>
<thead>
<tr class="header">
<th>Data/Process</th>
<th>What is exchanged</th>
<th>When</th>
</tr>
</thead>
<tbody>
<tr>
<td>Customer data</td>
<td><p>Data about the customer such as</p>
<ul>
<li>Invoice and buy address</li>
<li>list of delivery addresses</li>
<li>status</li>
<li>customer group</li>
<li>creditlimit</li>
<li>contact data</li>
</ul></td>
<td>The data for a customer will be fetched from the ERP when the user logs in and has a customer number</td>
</tr>
<tr>
<td>Product data</td>
<td>Products and product groups</td>
<td>During the import. The import will be initiated e.g. every night or even more often. This data is usually provided using an export (XML,CSV,Json)</td>
</tr>
<tr>
<td>Prices</td>
<td><p>List price and volume prices</p>
<p>Individual prices</p></td>
<td><p>List prices will be exchanged during the product import</p>
<p>Individual prices will be fetched from the ERP when a customer is logged in and has a customer number. In that case the shop requests prices in realtime from the ERP. It can be defined by project in which cases prices are requested from the ERP (.e.g product detail page, basket and checkout)</p></td>
</tr>
<tr>
<td>Orders</td>
<td>Address data, Delivery address, products and customer number</td>
<td><p>When the customer has ordered the order will be send immediately to the ERP system. If a electronic payment is involved the order will be placed when the payment provider has acknowledged the payment transaction.</p>
<p><br />
</p></td>
</tr>
<tr>
<td>Documents</td>
<td>Invoices, orders, delivery notes, credit memos</td>
<td>The addon order history requests such documents in real time from the ERP. This ensures that the customer will see all documents even if an order has been placed by phone or fax.</td>
</tr>
</tbody>
</table>

## Further information

The documentation provides more information about this topic:

  - [How to create a new ERP message (example: ItemTransfer)](23560266.html)
  - About UBL and how to add new fields:  [Usage of UBL](Usage-of-UBL_23560640.html)
  - [Web-Connector Configuration](Web-Connector-Configuration_23560325.html)

## Attachments:

![](images/icons/bullet_blue.gif) [image2019-2-4\_19-48-45.png](attachments/23560631/23566389.png) (image/png)  
