# Customer profile data listeners 

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
Classes

</th>
<th><div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
Description

</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>\Silversolutions\Bundle\EshopBundle\EventListener\CustomerProfileData\ListenerInterface</code></td>
<td>The interface defines, which methods all listeners must have (e.g. <code>onPreFetch()</code>, <code>onPostFetch()</code> events)</td>
</tr>
<tr>
<td><code>\Silversolutions\Bundle\EshopBundle\EventListener\CustomerProfileData\EzErpListener</code></td>
<td><p>Concrete event listener implementation. Provides ERP related listeners, e.g. for ERP response success or fail.</p>
<p>onPostErpCustomerFetchFail():</p>
<ul>
<li>Event: ses_ez_erp_customer_profile_data_post_erp_customer_fetch_fail</li>
<li>Loads customer data from the respective eZ User content object if it could not be retreived from the ERP.</li>
</ul>
<p>onPreFetch():</p>
<ul>
<li>No implementation</li>
</ul>
<p>onPostFetch():</p>
<ul>
<li>No implementation</li>
</ul></td>
</tr>
</tbody>
</table>
