#  Customer profile data controller 

## Actions

`\Silversolutions\Bundle\EshopBundle\Controller\CustomerProfileDataController`

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Action
</th>
<th>Route</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>showDetailAction()</code></td>
<td><code>silversolutionsCustomerDetail</code></td>
<td>Renders the <a href="#Customerprofiledatacontroller-profile">profile detail page</a></td>
</tr>
<tr>
<td><pre><code>addressBookAction()</code></pre></td>
<td><pre><code>silversolutions_address_book_list</code></pre></td>
<td>Renders the address book (list with delivery addresses coming from ERP)</td>
</tr>
<tr>
<td><pre><code>addressBookDeleteAction()</code></pre></td>
<td><pre><code>silversolutions_address_book_delete</code></pre></td>
<td>Removes given delivery address from ERP and customer pofile data</td>
</tr>
<tr>
<td><code>logoutAction()</code></td>
<td><code>silversolutionsCustomerLogout</code></td>
<td>Unsets all profile data within the session, logs out the user and redirects to previous page</td>
</tr>
</tbody>
</table>

## Profile detail page

![](attachments/23560596/23563237.png)

User can manage his data profile data in this page.

In the address list user can

  - update existing delivery address
  - create a new delivery address
  - remove one existing delivery address

All changes in the delivery addresses will be send to the ERP.

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2017-02-13 um 14.24.25.png](attachments/23560596/23563237.png) (image/png)  
