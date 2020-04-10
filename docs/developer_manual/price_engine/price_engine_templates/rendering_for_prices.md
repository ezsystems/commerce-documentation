# Rendering for prices

## Render PriceField

To render the `customerPrice` (instance of `PriceField`) of the `ProductNode`, you might use the Twig function `ses_render_field()` within the template `MyTestBundle::product.html.twig.`

``` 
{{ ses_render_field(catalogElement, 'customerPrice') }}
```

Following optional render parameters are available for a `PriceField`:

##### outputPrice

Enables actions on the output of the price value

|Parameter|Description|Default|
|--- |--- |--- |
|id|Optional string of ID to use for price|"" (undefined)|
|cssClass|Optional string of CSS class(es) to use for price|"" (undefined)|
|locale|Two digit locale code (e.g.: "en", "us", "de")|"en"|
|currency|Three digit currency code (e.g.: EUR, GBP, USD)|price.currency|
|property|Property of price field used for outputted value|price.price|
|raw|Price is outputted without any HTML tags, if true|false (undefined)|

##### vatLabel

Enables actions on the optional VAT label

|Parameter|Description|Default|
|--- |--- |--- |
|id|Optional string of ID to use for VAT label|"" (undefined)|
|show|VAT label is shown, if true|false (undefined)|
|cssClass|Optional string of CSS class(es) to use for VAT label|"" (undefined)|
|text|Override of the outputted text|Default text depending on price.isVatPrice ("Including VAT" or "Excluding VAT")|
|raw|VAT label is outputted without any HTML tags, if true|false (undefined)|

##### schema

Enables to ouput schema infos

If set then a schema `itemprop="price"` will be used:

```
<span itemprop="price" content="1865.00">1.865,00&nbsp;€</span>
```

Following example would output the value of property "`priceExclVat`" (property: "`priceExclVat`") from the price field in German (locale: "`de`") standard format with enforced used of the Euro sign (currency: "`EUR`"). The CSS class "`price_med`" is set to the price `<p>` tag. Furthermore a VAT label is shown below the price (show: `true`) with defined text "`Excluding VAT`" and CSS classes "`price_info`" and "`smaller`" to the VAT `<p>` tag.

``` html+twig
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
