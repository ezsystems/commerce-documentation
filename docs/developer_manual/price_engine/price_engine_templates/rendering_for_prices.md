# Rendering for prices 

## Render PriceField

To render the `customerPrice` (instance of `PriceField`) of the `ProductNode`, you might use the Twig function `ses_render_field()` within the template `MyTestBundle::product.html.twig.`

``` 
{{ ses_render_field(catalogElement, 'customerPrice') }}
```

Following optional render parameters are available for a `PriceField`:

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Options</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>outputPrice</code></td>
<td>Enables actions on the output of the price value</td>
<td>
<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Default</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>id</code></td>
<td>Optional string of ID to use for price</td>
<td><code>""</code> (undefined)</td>
</tr>
<tr>
<td><code>cssClass</code></td>
<td>Optional string of CSS class(es) to use for price</td>
<td><code>""</code> (undefined)</td>
</tr>
<tr>
<td><code>locale</code></td>
<td>Two digit locale code (e.g.: "en", "us", "de")</td>
<td><code>"en"</code></td>
</tr>
<tr>
<td><code>currency</code></td>
<td>Three digit currency code (e.g.: EUR, GBP, USD)</td>
<td><code>price.currency</code></td>
</tr>
<tr>
<td><code>property</code></td>
<td>Property of price field used for outputted value</td>
<td><code>price.price</code></td>
</tr>
<tr>
<td><code>raw</code></td>
<td>Price is outputted without any HTML tags, if <code>true</code></td>
<td><code>false</code> (undefined)</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td><code>vatLabel</code></td>
<td>Enables actions on the optional VAT label</td>
<td>
<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Default</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>id</code></td>
<td>Optional string of ID to use for VAT label</td>
<td><code>""</code> (undefined)</td>
</tr>
<tr>
<td><code>show</code></td>
<td>VAT label is shown, if <code>true</code></td>
<td><code>false</code> (undefined)</td>
</tr>
<tr>
<td><code>cssClass</code></td>
<td>Optional string of CSS class(es) to use for VAT label</td>
<td><code>""</code> (undefined)</td>
</tr>
<tr>
<td><code>text</code></td>
<td>Override of the outputted text</td>
<td><p>Default text depending on <code>price.isVatPrice</code> ("Including VAT" or "Excluding VAT")</p></td>
</tr>
<tr>
<td><code>raw</code></td>
<td>VAT label is outputted without any HTML tags, if <code>true</code></td>
<td><code>false</code> (undefined)</td>
</tr>
</tbody>
</table>
</td>
</tr>
<tr>
<td>schema</td>
<td>enables to ouput schema infos</td>
<td><div class="content-wrapper">
<p>If set then a schema</p>
<pre><code>itemprop=&quot;price&quot; </code></pre>
<p>will be used:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;span itemprop=&quot;price&quot; content=&quot;1865.00&quot;&gt;1.865,00&amp;nbsp;€&lt;/span&gt;</code></pre>
</td>
</tr>
</tbody>
</table>

Following example would output the value of property "`priceExclVat`" (property: "`priceExclVat`") from the price field in German (locale: "`de`") standard format with enforced used of the Euro sign (currency: "`EUR`"). The CSS class "`price_med`" is set to the price `<p>` tag. Furthermore a VAT label is shown below the price (show: `true`) with defined text "`Excluding VAT`" and CSS classes "`price_info`" and "`smaller`" to the VAT `<p>` tag.

``` 
{{ ses_render_field(
    catalogElement,
    'customerPrice',
    {
        'outputPrice': {'property': 'priceExclVat', 'cssClass': 'price_med', 'currency': 'EUR', 'locale': 'de'},
        'vatLabel': {'show': true, 'cssClass': 'price_info smaller', 'text': 'Excluding VAT'|trans}
    }
)
}}
```

## Attachments:

![](images/icons/bullet_blue.gif) [Screenshot from 2015-06-17 14:47:57.png](attachments/23560289/23563826.png) (image/png)  
