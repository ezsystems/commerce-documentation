#  Orderhistory - Templates 

### Templates list:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Path</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/list.html.twig</code></pre></td>
<td>Renders the list view of requested documents</td>
</tr>
<tr>
<td><pre><code>vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/list_table.html.twig</code></pre></td>
<td><p>Renders the table with list of requested documents</p>
<p>Included in</p>
<pre><code>views/OrderHistory/list.html.twig</code></pre></td>
</tr>
<tr>
<td><pre><code>vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/detail.html.twig</code></pre></td>
<td>Renders the detail view of requested document</td>
</tr>
<tr>
<td><pre><code>vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/header_default.html.twig</code></pre></td>
<td><p>Renders the header information for document detail</p>
<p>Included in</p>
<pre><code>views/OrderHistory/detail.html.twig</code></pre></td>
</tr>
<tr>
<td><pre><code>vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/fields.html.twig</code></pre></td>
<td><p>Contents blocks that renders content of requested field for columns (defined in the configuration).</p>
<p>Included in</p>
<pre><code>views/OrderHistory/Components/list_table.html.twig</code></pre>
<pre><code>views/OrderHistory/detail.html.twig</code></pre></td>
</tr>
<tr>
<td><pre><code>vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/user_menu.html.twig</code></pre></td>
<td>See <a href="Customers---FAQ_23560609.html">User menu</a></td>
</tr>
</tbody>
</table>

### Related custom Twig modifiers/functions/etc/:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Twig filter</th>
<th>Description</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>&#39;ses_erp_to_default&#39;</code></pre></td>
<td><pre><code>converts the ERP date into default date format</code></pre></td>
<td><pre><code>{{ &#39;Order at:&#39;|st_translate }} {{ response.OrderReference.IssueDate.value|ses_erp_to_default }}</code></pre></td>
</tr>
<tr>
<td><pre><code>&#39;ses_to_float&#39;</code></pre></td>
<td><pre><code>converts given value to valid float
(also strings that are using comma instead of dot)</code></pre></td>
<td><pre><code>{{ &#39;VAT:&#39;|st_translate }} {{ vat.TaxAmount.value|ses_to_float|price_format }}</code></pre></td>
</tr>
</tbody>
</table>

### Related routes:

``` 
siso_order_history_list:
    path:     /orderhistory
    defaults: { _controller: SisoOrderHistoryBundle:OrderHistory:list }

siso_order_history_detail:
    path:     /orderhistory/detail/{documentType}/{id}
    defaults: { _controller: SisoOrderHistoryBundle:OrderHistory:detail } 
```
