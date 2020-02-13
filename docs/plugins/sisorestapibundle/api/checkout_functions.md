# Checkout functions 

## getBasketPaymentMethods

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/checkout/payment-methods (GET)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>returns available payment options for current basket</p>
<p>Parameter based default implementation can be replaced via overridden service ID</p>
<p>standard service id: 'siso_rest_api.payment_methods_service'</p></td>
</tr>
<tr class="odd">
<td>Request</td>
<td><div class="content-wrapper">
<p>empty</p>
<p><br />
</p>
</td>
</tr>
<tr class="even">
<td>Response</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;PaymentMethodDataResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PaymentMethodDataResponse+json&quot;,
        &quot;paymentMethods&quot;: {
            &quot;paypal&quot;: &quot;paypal&quot;,
            &quot;invoice&quot;: &quot;invoice&quot;
        },
        &quot;defaultMethod&quot;: &quot;invoice&quot;
    }
}</code></pre>
</td>
</tr>
</tbody>
</table>

## getBasketShippingMethods

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 85%" />
</colgroup>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/checkout/shipping-methods (GET)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>returns available delivery options for current basket</p>
<p>Parameter based default implementation can be replaced via overridden service ID</p>
<p>standard service id: 'siso_rest_api.shipping_methods_service'</p></td>
</tr>
<tr class="odd">
<td>Request</td>
<td><div class="content-wrapper">
<p>empty</p>
<p><br />
</p>
</td>
</tr>
<tr class="even">
<td>Response</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;ShippingMethodDataResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ShippingMethodDataResponse+json&quot;,
        &quot;shippingMethods&quot;: {
            &quot;LIEFERUNG&quot;: &quot;standard_mail&quot;,
            &quot;mail&quot;: &quot;mail&quot;,
            &quot;expressDel&quot;: &quot;express_delivery&quot;
        },
        &quot;defaultMethod&quot;: &quot;LIEFERUNG&quot;
    }
}</code></pre>
</td>
</tr>
</tbody>
</table>
