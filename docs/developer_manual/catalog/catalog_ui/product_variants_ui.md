# Product variants UI

## Templates

A VariantProductNode has an own template, that is set in the configuration. It is used to render the full view of a product. 

silver.eshop.yml

``` 
silver_eshop.default.catalog_template.VariantProductNode: Catalog:product_variants.html.twig
```

## Twig filters

The data for the variants are provided by 2 twig filters. 

### `sort_characteristics()`

Sorting characteristics for catalog element type (like unit, color, weight etc)

``` html+twig
{% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement.type) %}
```

To change the order for catalog type, there is configuration that will sort in given order:

`silver.eshop.yml`

``` yaml
silver_eshop.default.catalog_type:
    vegetable:
        characteristics_order: ["1","2","3"],
```

If you want to change the order of characteristics manually (e.g. first unit and then color) you can pass the order as well:

``` html+twig
{% set order = ['2','1'] %}
{% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement, order) %}
```

### `sort_characteristic_codes()`

Sort specific characteristic codes (for codes like green, red, blue etc)

``` html+twig
// variantIndex eg. 1
{% set variantCharacteristic = variantCharacteristic|sort_characteristic_codes(variantIndex) %}
```

For every characteristic the is a way to determine sorting of codes that are in that characteristic

``` 
# by default the list of all variant codes is sorted alphabetically in the ASC order
# allowed values for order: ASC, DESC
# allowed values for sort: true, false
silver_eshop.default.sort_variant_codes:
    1:
        sort: false
#    2:
#        order: DESC
```
