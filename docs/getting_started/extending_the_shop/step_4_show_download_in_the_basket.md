# Step 4 - Show download in the basket

!!! note "What we want to achieve"

    Display a notice in the basket if a product is a digital product. In thi case we will change the info about the available stock since downloads will never run out of stock.

## Override the template for the basket line

The basket is build using a block system. The template 

`blocks/basket.html.twig`

contains a list of blocks. One of the block is responsible for diplaying a product line in the basket.

Please copy the standard template to the new bundle: `src/DemoBundle/Resources/views/blocks/basket.html.twig`

``` 
cp vendor/silversolutions/silver.e-shop//src/Silversolutions/Bundle/EshopBundle/Resources/views/blocks/basket.html.twig app/Resources/views/blocks/basket.html.twig
```

### Adapt the block `basket_part_lines`

``` html+twig
{% block basket_part_lines %}
    {% if ses.profile.sesUser.isPriceInclVat is same as(false) %}
        {% set showInclVat = false %}
    {% else %}
        {% set showInclVat = true %}
    {% endif %}
     .......
    {% if stockColumn %}
    <div class="large-{{ stockColumnSize }} columns text-center text-center stock">
        <div class="row u-margin-top-1x-on-small u-margin-top-1x-on-medium">
            <div class="small-6 columns hide-for-large-up text-left">
                <label class="u-no-margin"><strong>{{ 'Stock'|st_translate }}: </strong></label>
            
            <div class="small-6 large-12 columns small-text-right large-text-center c-basket__stock">
                {% if product.dataMap.isDownload|default(false) == true %}
                        <i class="fa fa-2x fa-arrow-circle-o-down" aria-hidden="true"></i>
                {% else %}
                    {{ stockField }}
                {% endif %}

{% endif %}

```

### Additional product fields and basket

When a product is added to the basket, the catalog element is serialized and stored in the basket line. Not all additional product data will be automatically stored\! If you want to store and access your additional product attributes in the basket without fetching the product again, you can adapt the configuration:

``` yaml
# Defines catalog element attributes that should be stored in a basket line.
siso_basket.default.stored_catalog_element_attributes:
    Silversolutions\Bundle\EshopBundle\Product\OrderableProductNode:
        baseAttributes:
            - customerPrice
            .....
            - section
        dataMapAttributes: [discontinued,isDownload,downloadFile]
```

In the basket you can access the attributes from the basket line:

``` html+twig
{% set basket = ses_basket() %}
{% for line in basket.lines %}
    {% if line.catalogElement|default is not empty and line.catalogElement.dataMap.isDownload|default is not empty %}
         
    {% endif %}
{% endfor %}
```
