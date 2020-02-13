# Customer Center - Templates 

### Templates list:

<table>
<thead>
<tr class="header">
<th>Path</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>Default path: vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views/CustomerCenter</code></pre></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><pre><code>index.html.twig</code></pre></td>
<td>entry page for customer center</td>
</tr>
<tr class="odd">
<td><pre><code>Basket/messages.html.twig</code></pre></td>
<td>subtemplate that will render an error message, if user exceeds his budget on the basket show page</td>
</tr>
<tr class="even">
<td><pre><code>Basket/remove_basket.html.twig</code></pre></td>
<td>subtemplate that will render the button to delete the current basket on the basket show page</td>
</tr>
<tr class="odd">
<td><pre><code>Email/approve_basket.html.twig</code></pre></td>
<td>html content for the email that is send to the approver to inform him about new approval request</td>
</tr>
<tr class="even">
<td><pre><code>Email/approve_basket.html.txt</code></pre></td>
<td>text content for the email that is send to the approver to inform him about new approval request</td>
</tr>
<tr class="odd">
<td><pre><code>Email/reject_basket.html.twig</code></pre></td>
<td>html content for the email that is send to buyer when approver rejected his basket</td>
</tr>
<tr class="even">
<td><pre><code>Email/reject_basket.html.txt</code></pre></td>
<td>text content for the email that is send to buyer when approver rejected his basket</td>
</tr>
<tr class="odd">
<td><pre><code>Email/new_account_pw.html.twig</code></pre></td>
<td>html content for the email that is send to user if main contact has created a shop account for him</td>
</tr>
<tr class="even">
<td><pre><code>Email/new_account_pw.html.txt</code></pre></td>
<td>text content for the email that is send to user if main contact has created a shop account for him</td>
</tr>
<tr class="odd">
<td><pre><code>Approver/send_to_approver.html.twig</code></pre></td>
<td>page that is displayed for buyer after he went through the approval process</td>
</tr>
<tr class="even">
<td><pre><code>Approver/reject_basket.html.twig</code></pre></td>
<td>page for the approver where he can reject the basket</td>
</tr>
<tr class="odd">
<td><pre><code>Approver/buyer.html.twig</code></pre></td>
<td>renders entry page for buyer with the approval requests</td>
</tr>
<tr class="even">
<td><pre><code>Approver/approver.html.twig</code></pre></td>
<td>renders entry page for approver with the approval requests</td>
</tr>
<tr class="odd">
<td><pre><code>Form/add_user.html.twig</code></pre></td>
<td>renders the form for add known erp user to the shop</td>
</tr>
<tr class="even">
<td><pre><code>Form/edit_user.html.twig</code></pre></td>
<td>renders the form for edit user</td>
</tr>
<tr class="odd">
<td><pre><code>Form/request_user.html.twig</code></pre></td>
<td>renders the form for request new user</td>
</tr>
<tr class="even">
<td><pre><code>Form/base_form.html.twig</code></pre></td>
<td>base template that is renders the form data and is used in all Form/* templates</td>
</tr>
<tr class="odd">
<td><br />
</td>
<td><br />
</td>
</tr>
<tr class="even">
<td><pre><code>vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views/parts/user_menu.html.twig</code></pre></td>
<td>overrides user_menu.html.twig from silver-e.shop</td>
</tr>
</tbody>
</table>

### Related custom Twig modifiers/functions/etc/:

### Twig functions

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Twig function</th>
<th>Description</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>is_customer_center_active</code></pre></td>
<td><pre><code>returns true if customer center is enabled</code></pre></td>
<td><pre><code>{% set is_customer_center_active = is_customer_center_active() %}
{% if is_customer_center_active %}
...</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>ses_user_budget</code></pre></td>
<td><pre><code>returns user budget as an array </code></pre>
<pre><code>Example:
 array(
 &#39;budget_order&#39; =&gt; 100.00,
 &#39;budget_month&#39; =&gt; 400.00
)</code></pre></td>
<td><pre><code>{% set userBudget = ses_user_budget(basket.dataMap.buyerUserId) %}</code></pre>
<pre><code>{% if userBudget is iterable %}
    &lt;ul &gt;
        {% for userBudgetKey, userBudgetValue in userBudget %}
            &lt;li&gt;{{ userBudgetKey|st_translate }}: {{ userBudgetValue|price_format(ses.profile.sesUser.customerCurrency) }}&lt;/li&gt;
        {% endfor %}
    &lt;/ul&gt;
{% endif %}</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>is_customer_center_buyer</code></pre></td>
<td><pre><code>returns true if user is a customer center user and has the buyer role</code></pre></td>
<td><pre><code>{% set is_customer_center_buyer = is_customer_center_buyer() %}
{% if is_customer_center_buyer %}
...</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>is_customer_center_approver</code></pre></td>
<td><pre><code>returns true if user is a customer center user and has the buyer role</code></pre></td>
<td><pre><code>{% set customer_center_approver = is_customer_center_approver() %}
{% if customer_center_approver %}
...</code></pre></td>
</tr>
<tr class="odd">
<td><pre><code>is_customer_center_main_contact</code></pre></td>
<td><pre><code>returns true if user is a customer center user and has the buyer role</code></pre></td>
<td><pre><code>{% if is_customer_center_main_contact() %}
...</code></pre></td>
</tr>
<tr class="even">
<td><pre><code>ses_user_order_sum_last_month</code></pre></td>
<td><pre><code>returns the sum of user orders of the last month</code></pre></td>
<td><pre><code>{% set userOrderSum = ses_user_order_sum_last_month(basket.dataMap.buyerUserId) %}</code></pre></td>
</tr>
</tbody>
</table>

### Twig Tests

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Twif test</th>
<th>Description</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>customer_center_contact</code></pre></td>
<td><pre><code>returns true if given contact is already customer center contact</code></pre></td>
<td><pre><code>{% if contact is not customer_center_contact(customer_center_contacts) %}</code></pre>
<p>...</p></td>
</tr>
<tr class="even">
<td><pre><code>customer_center_contacts</code></pre></td>
<td><pre><code>returns true if all erp contacts are already customer center contacts</code></pre></td>
<td><pre><code>{% if erp_contacts is not empty
 and erp_contacts is not customer_center_contacts(customer_center_contacts)
%}
...</code></pre></td>
</tr>
</tbody>
</table>
