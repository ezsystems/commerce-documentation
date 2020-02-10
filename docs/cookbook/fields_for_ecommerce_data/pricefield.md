#  PriceField 

`PriceField` is the representative implementation of `AbstractField` for a **`Price`**.

A new `PriceField` can be created using the following data:

``` 
use Silversolutions\Bundle\EshopBundle\Content\Fields\PriceField;
use Siso\Bundle\PriceBundle\Model\Price;
 
// Usage: 
$price = new Price(
    array(
        'price' => 99.99,
        'isVatPrice' => true,
        'vatCode' => 19.0,
        'currency' => 'EUR',
        'source' => 'ERP',
    )
);
$priceField = new PriceField(array('price' => $price));
```

#### Rendering

Please refer to [Rendering for prices](Rendering-for-prices_23560289.html) to see the possibilities of outputting a PriceField using the `ses_render_field()` function.

It is also possible to render a priceField with a twig function **ses\_render\_price().** The difference see in the table below:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Twig function</th>
<th>Paramaters</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>ses_render_field()</code></td>
<td><p>$catalogElement</p>
<p>string $fieldIdentifier</p>
<p>array $<a href="Rendering-for-prices_23560289.html">params</a></p></td>
<td><p>more general -</p>
<p>renders also other FieldInterface $fields from $catalogElement</p>
<p>like TextBlockField, ImageField, PriceField</p></td>
</tr>
<tr>
<td><strong><code>ses_render_price()</code></strong></td>
<td><p>$catalogElement</p>
<p>PriceField $priceField</p>
<p>array $<a href="Rendering-for-prices_23560289.html">params</a></p></td>
<td><p>renders only PriceField $price</p>
<p> </p></td>
</tr>
</tbody>
</table>
