#  Vouchers - Templates 

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
<td>Silversolutions/Bundle/EshopBundle/Resources/views/Basket/show.html.twig</td>
<td><p>Block with vouchers is included here</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>{% block vouchers %}
...
{% endblock %}</code></pre>

</td>
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
<th>Twig functions</th>
<th>Description</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>ses_contains_basket_vouchers</code></pre></td>
<td>Returns true if basket contains some vouchers</td>
<td><pre><code>{% if ses_contains_basket_vouchers(basket) %}
  &lt;a class=&quot;button&quot; href=&quot;{{ path(&#39;siso_remove_voucher&#39;) }}&quot;&gt;
    &lt;i class=&quot;fa fa-times&quot;&gt;&lt;/i&gt; {{ &#39;button.remove_vouchers&#39;|st_translate }}
  &lt;/a&gt;
{% endif %}</code></pre></td>
</tr>
<tr>
<td><pre><code>ses_get_basket_vouchers</code></pre></td>
<td>Returns list of vouchers from the basket</td>
<td><pre><code>{% set vouchers = ses_get_basket_vouchers(basket) %}
{% for voucher in vouchers %}   
  &lt;p&gt;{{ &#39;common.voucher_redeemed&#39;|st_translate }} {{ voucher }}&lt;/p&gt;
  &lt;a class=&quot;button&quot; href=&quot;{{ path(&#39;siso_remove_voucher&#39;, {&#39;voucherNumber&#39; : voucher}) }}&quot;&gt;
    &lt;i class=&quot;fa fa-times&quot;&gt;&lt;/i&gt; {{ &#39;button.remove_voucher&#39;|st_translate }}
  &lt;/a&gt;  
{% endfor %}</code></pre></td>
</tr>
</tbody>
</table>

### Related routes:

``` 
siso_redeem_voucher:
    pattern: /voucher/redeem
    defaults: { _controller: SisoVoucherBundle:Voucher:redeemVoucher }

siso_remove_voucher:
    pattern: /voucher/remove/{voucherNumber}
    defaults:
        _controller: SisoVoucherBundle:Voucher:removeVoucher
        voucherNumber: null 
```
