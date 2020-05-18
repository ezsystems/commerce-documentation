# Product variants UI

## Templates

A `VariantProductNode` has its own template defined in the configuration.
It is used to render the full view of a product. 

``` yaml
silver_eshop.default.catalog_template.VariantProductNode: Catalog:product_variants.html.twig
```

## Twig filters

The data for the variants is provided by two Twig filters. 

### `sort_characteristics()`

`sort_characteristics()` sorts characteristics for a catalog element type (like unit, color, weight etc.):

``` html+twig
{% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement.type) %}
```

You can change the order in which characteristics are displayed per catalog type:

``` yaml
silver_eshop.default.catalog_type:
    vegetable:
        characteristics_order: ["1","2","3"],
```

If you want to change the order of characteristics manually (e.g. unit first and then color) you can pass the order in the template as well:

``` html+twig
{% set order = ['2','1'] %}
{% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement, order) %}
```

### `sort_characteristic_codes()`

`sort_characteristic_codes()` sorts specific characteristic codes (for codes like green, red, blue etc.):

``` html+twig
// variantIndex, e.g. 1
{% set variantCharacteristic = variantCharacteristic|sort_characteristic_codes(variantIndex) %}
```

For every characteristic you can determine the sorting of codes that are in that characteristic:

``` 
# by default the list of all variant codes is sorted alphabetically in the ASC order
# allowed values for order: ASC, DESC
# allowed values for sort: true, false
silver_eshop.default.sort_variant_codes:
    1:
        sort: false
    2:
        order: DESC
```
