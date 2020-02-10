#  Wishlist & Stored Basket - Templates 

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
<td><pre><code>Basket/stored_baskets_list.html.twig</code></pre></td>
<td>list of all stored baskets</td>
</tr>
<tr>
<td><pre><code>Basket/show_stored_basket.html.twig</code></pre></td>
<td>entry page for wishlist and stored baskets. Based on the basket type it loads one of the templates listed below</td>
</tr>
<tr>
<td><pre><code>Basket/show_stored_basket_part.html.twig</code></pre></td>
<td>partial page responsible for rendering stored basket page</td>
</tr>
<tr>
<td><pre><code>Basket/show_wishlist_part.html.twig</code></pre></td>
<td>partial page responsible for rendering wishlist page</td>
</tr>
<tr>
<td><pre><code>Basket/messages.html.twig</code></pre></td>
<td>template with success/error/notice messages for baskets</td>
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
<td><pre><code>get_stored_baskets</code></pre></td>
<td><pre><code>returns stored baskets for current user</code></pre></td>
<td><pre><code>{% set storedBaskets = get_stored_baskets() %}</code></pre>
<pre><code>{% if storedBaskets|default is not empty %}</code></pre>
<pre><code>...</code></pre></td>
</tr>
</tbody>
</table>
