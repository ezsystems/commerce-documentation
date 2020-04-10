# VariantProductNode and OrderableVariantNode

## VariantProductNode

VariantProductNode can not be ordered and contains all possible variants for a product. It inherits from [ProductNode](23560315.html). It contains additional properties. These properties are automatically validated withing the constructor using the `validateProperties()` method.

| Identifier             | Type                                                                          | Description                          |
| ---------------------- | ----------------------------------------------------------------------------- | ------------------------------------ |
| priceRange             | PriceField\[2\]                                                               | contains the min and max PriceField  |
| variantCharacteristics | [VariantCharacteristicsInterface](SimpleVariantCharacteristics_23560236.html) | contains all variant characteristics |

### Rendering PriceRange

``` html+twig
{# render min price #}
{% set minPrice = catalogElement.priceRange.minPrice %}
{{ ses_render_price(catalogElement, minPrice,
      {'outputPrice': {'cssClass': 'price price_med'}})
}}

{# render max price #}
{% set maxPrice = catalogElement.priceRange.maxPrice %}
{{ ses_render_price(catalogElement, maxPrice,
      {'outputPrice': {'cssClass': 'price price_med'}})
}}
 
```

## OrderableVariantNode

To make the VariantProductNode orderable a service is used - [VariantService](Variant-Services_23560238.html), that creates an OrderableVariantNode from VariantProductNode and given variantCode
