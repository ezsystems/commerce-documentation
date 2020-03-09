# PriceField

`PriceField` is the representative implementation of `AbstractField` for a **`Price`**.

A new `PriceField` can be created using the following data:

``` php
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

|Twig function|Paramaters|Usage|
|--- |--- |--- |
|ses_render_field()|$catalogElement</br>string $fieldIdentifier</br>array $params|more general -</br>renders also other FieldInterface $fields from $catalogElement</br>like TextBlockField, ImageField, PriceField|
|ses_render_price()|$catalogElement</br>PriceField $priceField</br>array $params|renders only PriceField $price|
