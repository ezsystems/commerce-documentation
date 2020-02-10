#  ERP integration 

Advanced version only

The ERP Integration offers a set of business processes. It requires an interface to an ERP system.

silver.solutions offers web.connectors for 

  - SAP 
  - Microsoft Dynamics NAV and AX

 The product provides an open interface which can be adapted to other ERP systems as well.

## Supported business processes

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 32%" />
<col style="width: 52%" />
</colgroup>
<thead>
<tr class="header">
<th>Process</th>
<th>Details</th>
<th>Advantage</th>
</tr>
</thead>
<tbody>
<tr>
<td>Login for B2B customer</td>
<td>The shop updates customer data from the ERP in realtime</td>
<td><p>The shop is using the customer data from the ERP. Always up-to-date.</p>
<p>The customer is always using the same data regardless of the channel he is using.</p></td>
</tr>
<tr>
<td>Activate B2B account</td>
<td>The shop can validate an account request using the customer number and other parts of an invoice (e.g. invoice number).</td>
<td><p>Existing cstomers can create a shop account and get access within minutes which minimizes the internal work for the shop owner.</p></td>
</tr>
<tr>
<td>Realtime stock</td>
<td>The shop can request stock information in realtime.</td>
<td>The stock is updated in realtime.</td>
</tr>
<tr>
<td>Realtime prices</td>
<td>The prices used in the shop are coming directly from the ERP. The shop uses the business logic of the ERP.</td>
<td><p>Prices are always uptodate. Complex price rules are used without duplicating the logic.</p></td>
</tr>
<tr>
<td>Orders</td>
<td>Orders are directly transferred to the ERP.</td>
<td><p>The customer gets feedback (e.g. order number) in a few seconds.</p></td>
</tr>
<tr>
<td>Documents</td>
<td>The shop requests orders, delivery notes, invoices and credit memos.</td>
<td><p>The customer can get and print documents for all channels (online, offline orders).</p>
<p>Less risks since the data is not duplicated to the shop server.</p></td>
</tr>
<tr>
<td>Products</td>
<td>Some ERP systems provides methods to import product data from the ERP.</td>
<td>Automatic product Synchronisation</td>
</tr>
</tbody>
</table>

## Fallback scenario

The eZ Commerce Advanced version supports fallback scenarios for the most important processes in case the connection to the ERP is not available:

  - The latest customer data is cached after login and used if the ERP is not available after login
  - A fallback price engine is used in case the ERP is not available. The customer is informed that the prices and stock is not up-to-date
  - An order will be stored in the shop. A job will take care that the order is transmitted to the ERP when it is available again  

## Realtime stock information during checkout and in product detail page

eZ Commerce is able to request realtime stock information from the ERP to ensure that the products are available in the warehouse.

If the stock is lower than the quantity required by the customer an icon and a tooltip will be displayed. It is possible to display the real stock as a numeric value as well.

![](attachments/23560195/23562829.png)

## Configuration of price providers

The shop owner can define which system will be responsible for calculating prices. 

In B2B the ERP is often the leading system. The price provider "siso\_price.price\_provider.remote" is the ERP system. 

A fallback price provider (e.g. using imported prices) can be configured. It is use in case the ERP is not available.

![](attachments/23560195/23563011.png)

## Monitoring in the backend

### Analyse messages between ERP and eZ Commerce

![](attachments/23560195/23562341.png)

## Show performance

The performance monitor allows to find and identify bottlenecks between eZ Commerce and the ERP system. 

It allows to analyse the communication for a given time period. The view can be filtered by a time period and allows to identify the message which are performing good or not good. 

![](attachments/23560195/23562356.png)

## ERP connectivity

An ERP system can be connected using different transport protocols:

  - Web-Services (SOAP)
  - Rest

An integrated mapping system provides a powerful feature to map the data provided an ERP to the internal data structure used by eZ Commerce.

The shop is using the standard format UBL (universal business language) for the most used entities such as

  - addressdata
  - orders
  - invoices

All requests and responses to and from the ERP will be mapped using xslt mapping files. 

The mapping files can be overwritten in a project.

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-10-30\_20-1-20.png](attachments/23560195/23563014.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-10-30\_20-3-8.png](attachments/23560195/23563015.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-10-31\_13-7-59.png](attachments/23560195/23563011.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-11-13\_10-39-10.png](attachments/23560195/23562829.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-11-21\_9-25-8.png](attachments/23560195/23562341.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-11-21\_9-26-10.png](attachments/23560195/23562354.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-11-21\_9-29-26.png](attachments/23560195/23562356.png) (image/png)  
