#  Manage orders 

## Order management 

eZ Commerce provides a list of all orders in the backend: eZ Commerce -\> Order Management

![](attachments/23560399/23570784.png)

The shop owner can check, filter and export all the orders that were processed in the shop. Additionally he can see the invoice that was automatically generated by the shop for the order (if no ERP is connected).

### Filters offered 

![](attachments/23560399/23565206.png)

There are different filters to make it easier: 

  - Filter by **user ID**
  - Filter by **Customer number**
  - Filter by **date** : Possibility to filter by a period of time. By default it is set the period between the current date and one month in the past.
  - Filter by **State**: Select box with different states:
      - Confirmed
      - Payed
      - Advanced version only

  -   -   - Order failed - the order failed to be transferred to the ERP, the shop will retry to send it  
            
          - New - Baskets which are not yet sent  
            
          - Offered - Baskets in the payment process

  -   -   - Approval - Orders using an approval workflow (Module Customer Center)
          - Rejected - ORders refused by the approver (Module Customer Center)  

  - Filter by **Payment option**: Select box with different payment options (The list has to be configured in the section "Configuration settings" under "Checkout". 
    
      -     Paypal\_express\_checkout
      -     Invoice

  - Filter by **Payment transaction id.**

### The list of orders 

Fields that are displayed in the table.

![](attachments/23560399/23570943.png)

  - **Basket ID:** Id of the basket stored in the database.
  - **Date:** Date and time this basket has last been modified
  - Customer name: Name des Kunden
  - **Total:**  Total amount from the basket including VAT and shipping costs
  - **State:** state of the basket.
  - **Invoice**
  - Click on the plus ![(plus)](images/icons/emoticons/add.png) to see the following information:  
      - **Transaction ID:** If the order has been payed using an electronic payment the transactioncode will be displayed
      - **Payment options:** It will show the chosen payment option
      - **Invoice Adress**
      - **Delivery Adress**
      - **Products:** List of products (SKU) and the quantity

## With ERP connection only  
  
When an ERP system is used the orders will be transferred to the ERP. When an ERP is connected the shop does not create invoices, so no invoice will be available in the order management. Nevertheless the online orders can be checked in this section. 

In addition there is one important service functionality - the Lost Orders.

### Lost orders 

**What happens when an order cannot be transferred to the ERP**

Since the ERP system might not be up and running eZ Commerce is able to store the orders and retransmit orders when the ERP is working.  The orders are stored in a database table and marked for retransmission. The shop records the number of retries. By default the maximum number is 3 (siso\_checkout.default.max\_failed\_order).

The shop will send an email to the shop administrator describing details about the order:

![](attachments/23560399/23570942.png)

**Reason why an order could not be sent to the ERP**

There may be a lot of reasons for why an order has not been send to the ERP system. It depends on the ERP system if an approbate error message can be provided.

The error message varies as well. Some examples:

<table>
<thead>
<tr class="header">
<th>Reason</th>
<th>Details</th>
</tr>
</thead>
<tbody>
<tr>
<td>ERP offline</td>
<td><br />
</td>
</tr>
<tr>
<td>Connection from shop to ERP is not working</td>
<td><p>In this case a message such as</p>
<ul>
<li>"Cannot connect to host" </li>
<li>Send of message XXXXX failed (Navision)</li>
</ul>
<p>may be documented.</p></td>
</tr>
<tr>
<td>Customer blocked</td>
<td><br />
</td>
</tr>
<tr>
<td>Credit limit reached</td>
<td><br />
</td>
</tr>
<tr>
<td>Invalid fields in the order</td>
<td><p>The shops send e.g. a country code which isn´t setup in the ERP.</p>
<p><br />
</p></td>
</tr>
<tr>
<td>Invalid SKU</td>
<td>This might happen if a sku has been imported which is remove in the ERP system</td>
</tr>
</tbody>
</table>

**Manage lost orders**

eZ Commerce provides a list oder lost orders in the backend of the shop.  

Use the filter "State: Order failed" to see the lost orders.

Overview:  
![](attachments/23560399/23569460.png)  
**Customer number:** It gets the customer number from the basket ("basket.buyerParty.PartyIdentification\[0\].ID")

**Shop:** It shows which shop has been used by the customer (used for multishops only)

**Functions:** It shows a link toremove or to transfer the order to ERP if the order is a lost order

**  
**

**How transfer orders again**

Please choose an order which shall be transfered and click on "Transfer to ERP". The order will be send to the ERP and after a few seconds you will see an message:

Details of a Lost Order:  
![](attachments/23560399/23569462.png)  

<table>
<thead>
<tr class="header">
<th>No</th>
<th>Line</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Customer No</td>
<td><br />
</td>
</tr>
<tr>
<td>2</td>
<td>Transaction ID</td>
<td>if payment provider was involved</td>
</tr>
<tr>
<td>3</td>
<td>Payment</td>
<td>Chosen payment method</td>
</tr>
<tr>
<td>4</td>
<td>Invoice address</td>
<td><br />
</td>
</tr>
<tr>
<td>5</td>
<td>Delivery address</td>
<td><br />
</td>
</tr>
<tr>
<td>6</td>
<td>Products</td>
<td>Quantity and SKUs</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [lostorder-table.png](attachments/23560399/23563487.png) (image/png)  
![](images/icons/bullet_blue.gif) [lost\_order\_success.png](attachments/23560399/23563429.png) (image/png)  
![](images/icons/bullet_blue.gif) [failed\_order\_email\_admin.png](attachments/23560399/23563428.png) (image/png)  
![](images/icons/bullet_blue.gif) [order\_list.jpg](attachments/23560399/23563537.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [order\_list.jpg](attachments/23560399/23563536.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2017-05-10 um 08.26.45.png](attachments/23560399/23563615.png) (image/png)  
![](images/icons/bullet_blue.gif) [lost.png](attachments/23560399/23563616.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2017-10-13 um 07.50.21.png](attachments/23560399/23563648.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order\_management.png](attachments/23560399/23570970.png) (image/png)  
![](images/icons/bullet_blue.gif) [order\_management\_filter.png](attachments/23560399/23570971.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order Management.png](attachments/23560399/23570949.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order management 3.png](attachments/23560399/23570943.png) (image/png)  
![](images/icons/bullet_blue.gif) [failed\_order\_email\_admin 1.png](attachments/23560399/23570942.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order Management1.png](attachments/23560399/23569460.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order Management2.png](attachments/23560399/23569464.png) (image/png)  
![](images/icons/bullet_blue.gif) [Order Management2.png](attachments/23560399/23569462.png) (image/png)  
![](images/icons/bullet_blue.gif) [Filter order management.png](attachments/23560399/23565206.png) (image/png)  
![](images/icons/bullet_blue.gif) [order\_management1.png](attachments/23560399/23570784.png) (image/png)  