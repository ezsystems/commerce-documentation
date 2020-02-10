#  Product comparison - Templates 

### Templates list:

Default path:

    vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Path
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>Comparison/comparison_list.html.twig</code></pre></td>
<td>entry page for comparison list</td>
</tr>
<tr>
<td><pre><code>Basket/messages.html.twig</code></pre></td>
<td>template with success/error/notice messages for baskets</td>
</tr>
<tr>
<td><pre><code>parts/user_menu.html.twig</code></pre></td>
<td>html content for right user menu navigation with links to comparison</td>
</tr>
</tbody>
</table>

### Related custom Twig modifiers/functions/etc/:

#### Twig functions

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Twig function
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
<th><div class="tablesorter-header-inner">
Usage
</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>ses_comparison_category</code></pre></td>
<td><p>Returns correct comparison category for the catalog element. This is a wrapper for <a href="Product-comparison---API_23560693.html#Productcomparison-API-Service">ComparisonServiceInterface::getComparisonCategory()</a></p></td>
<td><pre><code>{{ ses_comparison_category(catalogElement) }}</code></pre></td>
</tr>
</tbody>
</table>
