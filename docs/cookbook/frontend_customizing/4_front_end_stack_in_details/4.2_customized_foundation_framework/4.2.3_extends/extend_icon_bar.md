#  Extend - Icon Bar 

Extends: <http://foundation.zurb.com/sites/docs/v/5.5.3/components/icon-bar.html>

## What/why do we extend?

1.  In order to make it more custom look.

We use it in our header area for search, wish list, comparison and basket.

## Sass:

**File location**

``` 
scss/storm/_components.extend.icon-bar.scss
```

### Default settings:

``` 
$icon-bar-border-width: 0;
$icon-bar-border-style: solid;
$icon-bar-border-color: $primary-color;

$icon-bar-background: transparent;
$icon-bar-hover-background: transparent;

$icon-bar-wrapper-column-width: 5;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Icon Bar section.

## HTML

In order to get a flexible icon bar with possibilty to change icon order please use the code snippet below

``` 
<div class="c-icon-bar__wrapper">
  <div class="icon-bar four-up">
    <div class="item c-icon-bar__item">
      <a class="c-icon-bar__arrow" href="#" data-dropdown="dropdown-1">
        <i class="fa fa-cogs"></i>
        <label class="show-for-large-up">Icon bar item</label>
      </a>
      <div id="dropdown-1" class="f-dropdown content flexible no-arrow small text-left" data-dropdown-content>
        Content goes here

    <div class="item c-icon-bar__item">
      <a class="c-icon-bar__arrow" href="#" data-dropdown="dropdown-2">
        <i class="fa fa-cogs"></i>
        <label class="show-for-large-up">Icon bar item</label>
      </a>
      <div id="dropdown-2" class="f-dropdown content flexible no-arrow small text-left" data-dropdown-content>
        Content goes here

    <div class="item c-icon-bar__item">
      <a class="c-icon-bar__arrow" href="#" data-dropdown="dropdown-3">
        <i class="fa fa-cogs"></i>
        <label class="show-for-large-up">Icon bar item</label>
      </a>
      <div id="dropdown-3" class="f-dropdown content flexible no-arrow small text-left" data-dropdown-content>
        Content goes here

    <div class="item c-icon-bar__item">
      <a class="c-icon-bar__arrow" href="#" data-dropdown="dropdown-4">
        <i class="fa fa-cogs"></i>
        <label class="show-for-large-up">Icon bar item</label>
      </a>
      <div id="dropdown-4" class="f-dropdown content flexible no-arrow small text-left" data-dropdown-content>
        Content goes here

```

1.  Wrap the whole thing with *.c-icon-bar\_\_wrapper*
2.  Each item gets .*item* and .*c-icon-bar\_\_item classes*
3.  An anchor element gets .*c-icon-bar\_\_arrow class*
4.  Dropdown content despite all standard classes needs to get .*no-arrow* class in order to hide standard arrow from the default component settings. Class *.flexible* is optional and depends on the content that's inside the dropdown. You could replace with *.tiny, .small, .medium, .large, .xlarge* or *.full* in order to change the width of the dropdown.  

## Twig:

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig:236**

``` 
{% block header_feature_nav %}

  <div class="c-icon-bar__wrapper js-icon-bar">
    <div class="icon-bar four-up">
      <div class="item with-border hide-for-large-up">
        <a href="{{ path('siso_global_search') }}" data-dropdown="dropdown-search"
           data-options="is_hover: false" aria-controls="dropdown-search" aria-expanded="false">
          <i class="fa fa-search"></i>
        </a>

      {% block tools_preview %}
        <div class="item c-icon-bar__item">
          <a class="c-icon-bar__arrow" href="{{ path('silversolutions_basket_show') }}" data-dropdown="dropdown-tools">
            <i class="fa fa-cogs"></i>
            <label class="show-for-large-up">{{ 'common.toolkit'|st_translate }}</label>
          </a>
          <div id="dropdown-tools" class="f-dropdown content flexible no-arrow small text-left" data-dropdown-content>
            <div class="c-icon-bar__menu">
              <ul class="no-bullet u-no-margin-bottom">
                {{ ses_user_menu()|raw }}
              </ul>

      {% endblock %}

      {% block wishlist_preview %}
        {{ render_esi(
        controller(
        'SilversolutionsEshopBundle:Basket:showStoredBasketPreview',
        {'type': constant('Silversolutions\\Bundle\\EshopBundle\\Services\\BasketService::TYPE_WISH_LIST')}
        )
        ) }}
      {% endblock %}

      {% block comparison_preview %}
        {{ render_esi(
        controller(
        'SilversolutionsEshopBundle:Basket:showStoredBasketPreview',
        {'type': constant('Silversolutions\\Bundle\\EshopBundle\\Services\\BasketService::TYPE_COMPARISON')}
        )
        ) }}
      {% endblock %}

      {% block basket_preview %}
        {{ render_esi(
        controller(
        'SilversolutionsEshopBundle:Basket:showBasketPreview',
        {}
        ),
        {}
        ) }}
      {% endblock %}

{% endblock %}
```

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Basket/stored\_basket\_preview\_wish\_list.html.twig**

``` 
{% set wish_list = ses_wish_list() %}
<div
  class="item js-wishlist-flyout{% if ses.profile.sesUser.isAnonymous %} is-disabled{% endif %}{% if wish_list.lines.count == 0 %} is-inactive{% endif %}"
  {% if ses.profile.sesUser.isAnonymous %}data-tooltip
  title="{{ 'Please log in to use this feature'|st_translate }}"{% endif %}>
  <a
    href="{% if not ses.profile.sesUser.isAnonymous %}{{ path('silversolutions_wish_list_show') }}{% else %}#{% endif %}">

    <i class="fa fa-heart">
      <span class="label info round c-icon-bar__counter">{{ wish_list.lines.count|default(0) }}
    </i>
    <label class="show-for-large-up">{{ 'Wishlist'|st_translate }}</label>
  </a>

```

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Basket/stored\_basket\_preview\_comparison.html.twig**

``` 
{% set comparison = ses_total_comparison() %}
<div class="item js-comparison-flyout hide-for-medium-down{% if comparison.list is empty %} is-inactive{% endif %}">
    <a class="c-icon-bar__arrow hide-for-medium-down" href="{% if comparison.list is empty %}#{% else %}{{ path('silversolutions_comparison_list_show') }}{% endif %}"
       data-dropdown="dropdown-comparison" data-options="is_hover:true">
        <i class="fa fa-files-o">
            <span class="label info round c-icon-bar__counter">{{ comparison.total|default(0) }}
        </i>
        <label class="show-for-large-up">{{ 'Compare'|st_translate }}</label>
    </a>
    {% if comparison.list is not empty %}
        <div id="dropdown-comparison" class="f-dropdown no-arrow content" data-dropdown-content>
            <ul class="no-bullet text-left">
                {% for comparisonBasket in comparison.list %}
                    <li>
                        <a href="{{ path('silversolutions_comparison_list_show') }}#{{ comparisonBasket.id }}">
                            {{ comparisonBasket.name|st_translate }} ({{ comparisonBasket.total }})
                        </a>
                    </li>
                {% endfor %}
            </ul>
        
    {% endif %}

```

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Basket/basket\_preview.html.twig**

``` 
{% set basket = ses_basket() %}
<div class="item c-icon-bar__item--primary js-basket-flyout">
  <a class="c-icon-bar__arrow" href="{{ path('silversolutions_basket_show') }}" data-dropdown="dropdown-basket">
    <i class="fa fa-shopping-cart">
      <span class="label alert round c-icon-bar__counter">{{ basket.lines.count|default(0) }}
    </i>
    <label class="show-for-large-up">{{ 'Shopping basket'|st_translate }}</label>
  </a>

  {% if ses.profile.sesUser.isPriceInclVat %}
    {% set showInclVat = true %}
  {% else %}
    {% set showInclVat = false %}
  {% endif %}

  {% if basket.lines.count > 0 %}

    {% if showInclVat %}
      {% set totalPrice = basket.totalsSum.totalGross %}
    {% else %}
      {% set totalPrice = basket.totalsSum.totalNet %}
    {% endif %}

    <div id="dropdown-basket" class="f-dropdown medium no-arrow content text-left" data-dropdown-content>
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

      <hr>

      {# We need this var to align text in the flyout #}
      {% set isFlyout = true %}
      {% use "SilversolutionsEshopBundle:blocks:basket.html.twig" %}

      {{ block('basket_add_costs') }}

      <div class="row u-text-big">
        <div class="small-6 columns text-left">
          {{ 'Total'|st_translate }}:
        
        <div class="small-6 columns text-right">
          <strong>{{ totalPrice|price_format(basket.totalsSum.currency) }}</strong>

      <ul class="button-group even-2 stack-for-small u-margin-top-1x">
        <li class="u-full-width-on-small">
          <a href="{{ path('silversolutions_basket_show') }}" class="button u-color-white u-no-margin js-unbind">
            {{ 'Go to Basket'|st_translate }}
          </a>
        </li>
        <li class="u-full-width-on-small">
          <a href="{{ path('siso_checkout_homepage') }}" class="button u-color-white u-no-margin js-unbind">
            {{ 'Go to Checkout'|st_translate }}
          </a>
        </li>
      </ul>

  {% endif %}

```
