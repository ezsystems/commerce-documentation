#  Extend - Slick 

Extends and overrides some standard code from Slick slider

  - <https://github.com/kenwheeler/slick>
  - <http://kenwheeler.github.io/slick/>

## What/why do we extend?

1.  Use Font Awesome for prev/next arrows
2.  Style and position arrows a bit differently for homepage slider and sliders inside tabs

## Sass:

**File location**

``` 
scss/storm/_extend.components.slick.scss
```

### Default settings:

``` 
$slick-control-element-next: "\f105";
$slick-control-element-prev: "\f104";
$slick-control-element-color: $secondary-color;
$slick-icon-font-size: rem-calc(50);
$slick-icon-font-family: FontAwesome;
$slick-icon-z-index: 98;
$slick-sliders-icons-right: rem-calc(10);
$slick-sliders-icons-left: rem-calc(10);
$slick-sliders-icons-height: rem-calc(60);
$slick-sliders-icons-width: rem-calc(60);
$slick-sliders-icons-margin-top: rem-calc(-30);
$slick-slider-tabs-icon-next-right: rem-calc(-30);
$slick-slider-tabs-icon-next-left: rem-calc(-30);
```

We use font awesome icons you can get the icon "code" from <http://fontawesome.io/icons/>\!

In order to change settings in project find settings/\_storm.scss file in your project and find the Slick Slider section.

### Available custom classes

``` 
.slick_tabs_homepage_slider
.js-slick-main-homepage-slider
```

## Twig

### Slider inside tabs

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/slider.html.twig**

``` 
{% if productNodes|default is not empty %}
  {% if productNodes|length > 0 %}
    <div
      class="slick_tabs_homepage_slider u-no-margin-bottom u-no-margin-right-on-small u-no-margin-left-on-small js-product-list-wrapper"
      data-equalizer="description" data-options="equalize_on_stack: true">
      {% for catalogElement in productNodes %}
        {{ include('SilversolutionsEshopBundle:Catalog:sliderProductNode.html.twig') }}
      {% endfor %}
    
  {% endif %}
{% endif %}
```

### Main slider on the homepage

**File location**

``` 
{# "SesSliderBanner" block view template. #}
{# One block can contain one or several "block items". #}
{# Those items are passed from the PageController as "valid_items" and "valid_contentinfo_items" #}
{% if valid_items is not empty %}
    {% set displayBlockName = block.customAttributes.display_block_name %}
    {% if displayBlockName == 1 %}
        <h3>{{ block.name }}</h3>
    {% endif %}

    <div class="js-slick-main-homepage-slider u-no-margin-top u-no-margin-right-on-small u-no-margin-left-on-small">
    {# Now loop over block items to display them. #}
    {% for item in valid_items %}
        {{ render(
        controller(
        'ez_content:viewLocation',
        {
        'locationId': item.locationId,
        'viewType': 'block_item'
        }
        )
        ) }}
    {% endfor %}
    
{% endif %}
```
