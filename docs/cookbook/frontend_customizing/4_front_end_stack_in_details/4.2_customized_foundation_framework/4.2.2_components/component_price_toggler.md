# Component - Price Toggler

Component for the price toggler. We use price toggler to show/hide prices in multiple places in the shop.

It suppose to be used by the shop managers or other employees to hide the price when they have a customer on site in the shop.

## Sass

**File location**

``` 
scss/storm/_components.price-toggler.scss
```

## Twig

Make sure the toggle button is available. We suggest to place it in `src/Silversolutions/Bundle/EshopBundle/Resources/views/Components/user_menu.html.twig`. 

You can have multiple buttons if you want to control the visibility of different prices like user price, line price, special price, etc.

We make the toggle button visible only for logged in users. 

**src/Silversolutions/Bundle/EshopBundle/Resources/views/Components/user_menu.html.twig**

``` 
{% if ses_config_parameter('erp_connection', 'siso_core') %}
    {% if not ses.profile.sesUser.isAnonymous %}
        <li class="c-icon-bar__menu-item">
            <a class="c-icon-bar__menu-link js-cookie-price-btn" href="#" data-price-type="user" title="Toggle (on/off) price visbility" data-price-btn="user">
                <i class="fa fa-euro fa-fw c-icon-bar__menu-icon"></i> {{ 'label.pricetoggle'|st_translate }}
            </a>
        </li>
    {% endif %}
{% endif %}
```

**Note:** Pay attention to the data-price-btn="user" attribute here. You need to follow our naming convention if you want to make it work.

Next thing you need to do is to wrap the all the prices with a element that has data-price-wrap="user" attribute, e.g:

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/listProductNode.html.twig**

``` 
<div class="c-card__price-wrapper text-right" data-price-wrap="user">
  {% if ses.profile.sesUser.isPriceInclVat is sameas(false) %}
    {% set showInclVat = false %}
  {% else %}
    {% set showInclVat = true %}
  {% endif %}

  {% set priceProperty = 'priceExclVat' %}
  {% if showInclVat %}
    {% set priceProperty = 'priceInclVat' %}
  {% endif %}

  {% set labelParams = {
  'priceRange' : {
  'show': true,
  },
  'customerPrice': {
  'show': true,
  },
  'listPrice': {
  'show': true,
  }
  } %}

  {% set renderParams = {
  'outputPrice': {
  'property': priceProperty
  },
  'vatLabel': {
  'cssClass': '',
  'show': true,
  },
  'schema': true
  } %}
  {% if params.renderParams is defined %}
    {% set renderParams = params.renderParams %}
  {% endif %}

  {{ include('SilversolutionsEshopBundle:Catalog:Subrequests/product_price.html.twig'|st_resolve_template,
  {'renderParams': renderParams, 'labelParams': labelParams, 'displaySource' : false, 'displayShipping' : true}) }}

```

### List of twig templates that has user price show/hide implemented by default:

- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Basket/basket_preview.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/blocks/basket.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/listProductNode.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/sliderProductNode.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/Subrequests/product.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/parts/price_options.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/QuickOrder/quick_order_form.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Search/autosuggest/search_autosuggest_product_line.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Bestsellers/bestsellers_catalog.html.twig
- vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Bestsellers/bestsellers_box.html.twig
