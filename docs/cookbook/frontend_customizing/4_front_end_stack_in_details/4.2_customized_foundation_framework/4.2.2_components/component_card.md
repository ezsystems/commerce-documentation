# Component - Card

Mostly used for product items but can used as well for another type of content that contains body, title, description.

## Sass

**File location**

``` 
scss/storm/_components.card.scss
```

### Default settings:

``` 
$card-spacing-unit: ($rem-base/2); // 8px

$card-grid-items-small: 1;
$card-grid-items-medium: 2;
$card-grid-items-large: 3;
$card-grid-wide-items-large: 3;
$card-grid-wide-items-xlarge: 4;
$card-space-bottom: rem-calc(50);

$card-border-width: 1px;
$card-border-style: solid;
$card-border-color: $iron;
$card-position: relative;

$card-price-wrapper-padding-top: rem-calc($card-spacing-unit);

$card-button-group-position: absolute;
$card-button-group-right: rem-calc($rem-base + $card-spacing-unit);
$card-button-group-bottom: rem-calc($card-spacing-unit);
$card-button-disabled-hover-bg: $flat-concrete;

$card-title-font-size: rem-calc(16);
$card-title-padding: rem-calc($card-spacing-unit);
$card-title-margin: 0;
$card-title-overflow: hidden;
$card-title-text-overflow: ellipsis;
$card-title-link-color: $flat-peter-river;

$card-description-font-size: rem-calc(14);
$card-description-padding: rem-calc($card-spacing-unit);
$card-description-margin: rem-calc(0 0 $card-spacing-unit 0);
$card-description-overflow: hidden;
$card-description-last-paragraph-margin: rem-calc(0);

$card-sku-font-size: rem-calc(12);
$card-sku-padding: rem-calc($card-spacing-unit/2 $card-spacing-unit);
$card-sku-margin: 0;
$card-sku-overflow: hidden;
$card-sku-text-overflow: ellipsis;

$card-img-display: table;
$card-img-width: 100%;
$card-img-inline-display: inline-block;
$card-img-padding-top: rem-calc($card-spacing-unit);
$card-img-padding-bottom: rem-calc($card-spacing-unit);

$card-price-wrap-padding: rem-calc(0 $card-spacing-unit $card-spacing-unit $card-spacing-unit);
$card-price-wrap-line-height: normal;

$card-label-font-size: rem-calc(12);

$card-shipping-paragraph-display: inline;
$card-shipping-paragraph-font-size: 100%;
$card-shipping-paragraph-margin-bottom: 0;

$card-price-color: $primary-color;
$card-price-font-size: rem-calc(30);

$card-btn-secondary-wrap-width-small: 50%;
$card-btn-secondary-wrap-width-large: 25%;
$card-btn-secondary-padding-left: rem-calc(5);
$card-btn-secondary-padding-right: rem-calc(5);
$card-btn-secondary-bg-color: $flat-peter-river;
$card-btn-secondary-font-color: $white;
$card-btn-secondary-font-size: rem-calc(16);
$card-btn-primary-wrap-width: 50%;
$card-btn-primary-bg-color: $flat-peter-river;
$card-btn-primary-font-size: rem-calc(16);

$card-sidebar-bg-color: scale-color($white, $lightness: -5%);

$card-footer-padding: rem-calc($card-spacing-unit);
$card-footer-line-height: rem-calc(36);
$card-footer-bg-color: lighten($iron, 10%);
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Card section.

## HTML

``` 
<div class="c-card">
    <img class="c-card__image" src="path/to/image" alt="">
    <h3 class="c-card__title">Card title</h3>
    <p class="c-card__description">
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Corrupti ullam reiciendis, eum molestias commodi voluptatem doloribus consequatur tempore consequuntur asperiores nam, maiores nobis doloremque. Aperiam, enim, esse. Quibusdam, fuga vero.
    </p>
    <div class="c-card__price-wrapper">
        <div class="c-card__label c-card__label--price">Ihr Preis:
        <div class="c-card__price">68,90&nbsp;€
        <div class="c-card__label">Inklusive MwSt. (19%)
        <div class="c-card__label c-card__label--shipping">
            <p><a href="/Service/Kaufen/Versandkosten" target="_blank">zzgl. Versandkosten</a></p>

    <ul>
        <li class="c-card__button--secondary">
            <a href="#" class="button tiny disabled">
                <i class="fa fa-heart fa-lg fa-fw"></i>
            </a>
        </li>
        <li class="c-card__button--primary">
            <a href="#" class="button tiny js-add-to-basket">
                <i class="fa fa-cart-plus fa-lg fa-fw"></i>
            </a>
        </li>
    </ul>

```

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/listProductNode.html.twig**

``` 
 <li class="js-add-to-basket-parent">
  <form method="post" action="{{ path('silversolutions_add_to_basket') }}">
    <div class="c-card" data-equalizer="columns" data-options="equalize_on_stack: false">
      <div class="row">

        <div class="columns" data-grid-settings="small-3 columns" data-equalizer-watch="columns">
          {% set mainImage = ses_assets_main_image(catalogElement) %}
          <a href="{{ catalogElement.seoUrl }}">
            <img class="c-card__image" src="/{{ st_imageconverter(mainImage, 'thumb_medium') }}"
                 alt="{{ catalogElement.name }}">
          </a>

        <div class="columns" data-grid-settings="small-5 columns" data-equalizer-watch="columns">

          <h3 class="c-card__title" data-equalizer-watch="title">
            <a href="{{ catalogElement.seoUrl }}" title="{{ catalogElement.name }}">
              {{ catalogElement.name }}
            </a>
          </h3>

          <p class="c-card__description" data-equalizer-watch="description">
            {{ catalogElement.text }}
          </p>

        <div class="columns" data-grid-settings="small-4 columns" data-equalizer-watch="columns">

          <div class="c-card__price-wrapper text-right">
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
            'cssClass': 'u-inline'
            },
            'customerPrice': {
            'show': true,
            'cssClass': 'u-inline'
            },
            'listPrice': {
            'show': true,
            'cssClass': 'u-inline'
            }
            } %}

            {% set renderParams = {
            'outputPrice': {
            'cssClass': 'u-inline',
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

          <input type="hidden" name="ses_basket[0][quantity]" value="1">
          <input type="hidden" name="ses_basket[0][sku]" value="{{ catalogElement.sku }}">

          {{ include('SilversolutionsEshopBundle:Catalog:parts/productListButtonGroup.html.twig'|st_resolve_template) }}

  </form>
</li>
```

On listing pages like search or categories products are wrapped with `.c-card__list-wrapper`. It is important to wrap with this classes in order to get a grid looking list.

**HTML**

``` 
<ul class="c-card__list-wrapper">
    items partial goes here
</ul>
```

Default view (categories):

  - full width on small
  - two column on medium up (except list view)
  - three columns on large up

Search result page by default does not have sidebar so we can use a bit different approach. In order to make it full with we need to apply modifier class `c-card__list-wrapper-wide`:

**HTML**

``` 
<ul class="c-card__list-wrapper c-card__list-wrapper--wide">
    items partial goes here
</ul>
```

Wide view:

- all like default view
- additionally four columns on xlarge-up 
