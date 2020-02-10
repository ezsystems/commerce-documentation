#  Product Variants - UI 

## Templates

A VariantProductNode has an own template, that is set in the configuration. It is used to render the full view of a product. 

**silver.eshop.yml**

``` 
silver_eshop.default.catalog_template.VariantProductNode: Catalog:product_variants.html.twig
```

## Twig filters

The data for the variants are provided by 2 twig filters. 

1.  **sort\_characteristics()** - Sorting characteristics for catalog element type (like unit, color, weight etc)

    ``` 
    {% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement.type) %}
    ```

    To change the order for catalog type, there is configuration that will sort in given order:

    **silver.eshop.yml**

    ``` 
    silver_eshop.default.catalog_type:
        vegetable:
            characteristics_order: ["1","2","3"],
    ```

    If you want to change the order of characteristics manually (e.g. first unit and then color) you can pass the order as well:

    ``` 
    {% set order = ['2','1'] %}
    {% set sortedCharacterictics = catalogElement.variantCharacteristics.characteristics|sort_characteristics(catalogElement, order) %}
    ```

2.  **sort\_characteristic\_codes()** - sort specific characteristic codes (for codes like green, red, blue etc)

    ``` 
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

## Attachments:

![](images/icons/bullet_blue.gif) [Screenshot from 2015-04-16 12:32:32.png](attachments/23560478/23563306.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-04-16 12:38:22.png](attachments/23560478/23563305.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-04-16 12:38:43.png](attachments/23560478/23563319.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-04-16 12:38:49.png](attachments/23560478/23563320.png) (image/png)  
