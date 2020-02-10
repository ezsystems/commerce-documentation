#  Component - Flyout 

We use this component for any flyout type content e.g. basket flyout, etc. to limit the height. In our standard it works well with a dropdown component from Foundation, but it can be used to limit the height of any container.

## Sass

**File location**

``` 
scss/storm/_components.flyout.scss
```

### Default settings:

``` 
$flyout-max-height: rem-calc(300);
$flyout-overflow-y: auto;
$flyout-row-margin: 0;
$flyout-line-space-start: 0;
$flyout-line-space-end: 0;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Flyout section.

## HTML

``` 
<div class="c-flyout__content">
  Any content you want goes here

```

**Important class:**

  - c-flyout\_\_content

## Twig

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Basket/basket_preview.html.twig
 
<div id="dropdown-basket" class="f-dropdown medium content text-left" data-dropdown-content>
  <div class="c-flyout__content">
    {% for line in basket.lines %}
      {% if line.catalogElement|default is not empty %}
        {% set product = line.catalogElement %}
      {% else %}
        {% set product = ses_product({'sku': line.sku, 'variantCode': line.variantCode }) %}
      {% endif %}
      <div class="row u-margin-bottom-1x">
        <div class="small-7 columns text-left">
          {% if product|default is not empty %}
            <a href="{{ product.seoUrl }}">
              {{ line.quantity }} &times; {{ product.name }}
              {% if line.variantCode|default is not empty %}
                ({{ line.variantCode }})
              {% endif %}
            </a>
            {% else %}
            <a href="{{ path('silversolutions_basket_show') }}">
              {{ 'This product is not available any more! Please go to basket and remove it.'|st_translate }}
            </a>
          {% endif %}
        
        <div class="small-5 columns text-right">
          {% if showInclVat %}
            {{ line.linePriceAmountGross|price_format(line.currency) }}
            {% else %}
            {{ line.linePriceAmountNet|price_format(line.currency) }}
          {% endif %}

    {% endfor %}

```
