# VariantProductNode and OrderableVariantNode

## VariantProductNode

`VariantProductNode` cannot be ordered and contains all possible variants for a product. It inherits from [ProductNode](../productnode_and_orderableproductnode.md). It contains additional properties. These properties are automatically validated within the constructor using the `validateProperties()` method.

| Identifier             | Type          | Description                          |
| ---------------------- | ------------ | ------------------------------------ |
| `priceRange`             | `PriceField[2]`        | Contains the min and max `PriceField`  |
| `variantCharacteristics` | [VariantCharacteristicsInterface](simplevariantcharacteristics.md) | Contains all variant characteristics |

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

To make the `VariantProductNode` orderable the [VariantService](variant_services.md) is used.
It creates an `OrderableVariantNode` from `VariantProductNode` and given `variantCode`.
