#  StockField 

`StockField` is the representative implementation of `AbstractField` for a stock information.

The 'stockNumeric' information must be either *int* or *float*.

A new `StockField` can be created using the following data:

``` 
use Silversolutions\Bundle\EshopBundle\Content\Fields\StockField;
  
// Usage: 
$stockField = new StockField(array('stockNumeric' => 15));
```

### StockField Rendering

The StockField can be rendered via [twig helper](Twig-extension_23560696.html) ses\_render\_stock.

This method is rendering the StockField from a central template:

    Silversolutions/Bundle/EshopBundle/Resources/views/Fieldtypes/StockField.html.twig

The availability is displayed depending on given parameters. If the StockField is not defined (e.g. ERP is not responding), an information is displayed, that there is not availability information for this product.

``` 
{# display availability for the basket line, pass additionally the requested quantity in order to find out if the product is available in required amount #}
{% set field = line.remoteDataMap.stock is defined ? line.remoteDataMap.stock : null %}
    {{ ses_render_stock(field, {
                        'outputStock': {
                        'numeric': false,
                        'cssClass': 'availability'
                         },
                        'quantity' : line.quantity
     }) }}
{# display availability for the product detail page, set numeric to true, so the stock information will be displayed as a number: e.g. 54 items on Stock #}
{% set field = catalogElement.stock is defined ? catalogElement.stock : null %}
    {{ ses_render_stock(field, {
        'outputStock': {
        'numeric': true,
        'cssClass': 'availability'
        }
        })
    }}

```
