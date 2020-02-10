# Customer profile data events 

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
<td><code>\Silversolutions\Bundle\EshopBundle\Event\CustomerProfileData\CustomerProfileDataEventInterface</code></td>
<td>The general interface for any event which is thrown in customer profile data services is thrown.</td>
</tr>
<tr>
<td><code>\Silversolutions\Bundle\EshopBundle\Event\CustomerProfileData\AbstractCustomerProfileDataEvent</code></td>
<td>Abstract event for any customer profile data event provides helper methods like <code>setCustomerProfileData()</code> to make a profile available for an event listener.</td>
</tr>
<tr>
<td><code>\Silversolutions\Bundle\EshopBundle\Event\CustomerProfileData\EzErpCustomerProfileDataEvent</code></td>
<td><p>Concrete event which also provides the eZ user, e.g. for fallback purposes when the ERP did not respond (see <a href="Customer-profile-data-listeners_23560622.html">Customer profile data listeners</a>)</p>
<p>List of events, dispatched by \Silversolutions\Bundle\EshopBundle\Services\CustomerProfileData\EzErpCustomerProfileDataService:</p>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Event id</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_pre_fetch</code></pre></td>
<td><p>Thrown before any data is fetched from storage(s)</p></td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_post_fetch</code></pre></td>
<td>Thrown after all data is fetched from storage(s)</td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_pre_erp_customer_fetch</code></pre></td>
<td>Thrown before ERP customer data is fetched</td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_post_erp_customer_fetch_success</code></pre></td>
<td>Thrown after ERP customer data is successfully fetched</td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_post_erp_customer_fetch_fail</code></pre></td>
<td>Thrown after ERP customer data fetching failed</td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_pre_erp_contact_fetch</code></pre></td>
<td>Thrown before ERP contact data is fetched</td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_post_erp_contact_fetch_success</code></pre></td>
<td>Thrown after ERP contact data fetching successed</td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_post_erp_contact_fetch_fail</code></pre></td>
<td>Thrown after ERP contact data fetching failed</td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_pre_get_customer</code></pre></td>
<td>Thrown before customer data is returned</td>
</tr>
<tr>
<td><pre><code>ses_ez_erp_customer_profile_data_pre_save_customer</code></pre></td>
<td>Thrown before customer data is saved</td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>
