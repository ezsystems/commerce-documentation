# Component - Page Header

Component for the header area.

## Sass

**File location**

``` 
scss/storm/_components.page-header.scss
```

### Default settings:

``` 
$meta-nav-background: lighten($secondary-color, 10%);

$tools-wrap-background: $white;
$tools-wrap-border-color: $crumb-border-color;
$tools-wrap-border-style: solid;
$tools-wrap-border-width: rem-calc(1);
$tools-wrap-line-height: rem-calc(36);
$tools-wrap-padding-top: rem-calc(12);
$tools-wrap-padding-bottom: rem-calc(12);
$tools-wrap-line-height-large-up: normal;
$tools-wrap-padding-top-large-up: rem-calc(30);
$tools-wrap-padding-bottom-large-up: rem-calc(30);
$tools-wrap-border-bottom-width-large-up: 0;

$nav-wrap-position: relative;
$nav-wrap-position-right: rem-calc(10);
$nav-wrap-margin-top: rem-calc(-65);
$nav-wrap-padding-top: rem-calc(12);
$nav-wrap-padding-bottom: rem-calc(12);

$nav-wrap-border-color-on-large: $secondary-color;
$nav-wrap-border-style-on-large: solid;
$nav-wrap-border-width-on-large: rem-calc(1);
$nav-wrap-background-on-large: $white;
$nav-wrap-position-on-large: static;
$nav-wrap-margin-top-on-large: 0;
$nav-wrap-padding-on-large: 0;
$nav-wrap-background-contain-to-grid: $white;
$nav-wrap-border-color-fixed: $secondary-color;
$nav-wrap-border-style-fixed: solid;
$nav-wrap-border-width-fixed: rem-calc(1);

$nav-wrap-top-bar-background: none;
$nav-wrap-top-bar-overflow: visible;
$nav-wrap-top-bar-background-on-large: $white;
$nav-wrap-form-margin-top: rem-calc(12);
$nav-wrap-form-element-margin: 0;
$nav-wrap-form-button-background: $flat-belize-hole;
$nav-wrap-form-button-border-color: darken($flat-belize-hole, 5%);

$crumb-wrap-background: scale-color($white, $lightness: -5%);

$page-header-logo-position: relative;
$page-header-logo-index: 1;
$page-header-logo-display: inline-block;
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Page header section.

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig**

``` 
<div class="tools-wrap">
  <div class="row">
    <div class="small-6 large-3 columns">
      {% block header_content %}
        <div class="row collapse">
          <div class="small-2 columns show-for-medium-down">
            <a href="#js-main-offcanvas" role="button"
               class="left-off-canvas-toggle margin-right show-for-medium-down">
              <i class="fa fa-bars fa-lg"></i>
            </a>
          
          <div class="small-10 large-12 columns">
            <a href="{{ '/'|st_siteaccess_path }}" class="c-page-header__logo">
              <img src="{{ asset("bundles/silversolutionseshop/img/logo.png") }}" alt="Logo"/>
            </a>

      {% endblock %}
    
    {% block search %}
      <div class="large-4 columns hide-for-medium-down end">
        <form method="get" action="{{ path('silversolutions_search') }}">
          <div class="row collapse c-search">
            <div class="small-10 columns">
              <input type="text" class="c-search__text u-no-margin" id="siso_core_search_type_query"
                     name="siso_core_search_type[query]" required="required">
            
            <div class="small-2 columns">
              <button class="button postfix c-search__button u-no-margin u-no-padding" type="submit">
                <i class="fa fa-search fa-lg"></i>
              </button>

        </form>
      
    {% endblock %}

```

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig**

``` 
{% block main_nav %}
  <div class="nav-wrap">
    <div class="row">
      <div class="columns small-6 large-12 u-no-padding">
        <div class="sticky contain-to-grid">
          <nav class="top-bar c-megamenu--horizontal" data-topbar role="navigation" data-options="sticky_on: large">
            <ul class="title-area show-for-large-up">
              <li class="name"></li>
              <li class="toggle-topbar menu-icon"><a href="#">Menu</a></li>
            </ul>
            <section class="top-bar-section show-for-large-up">
                {{ render_esi(
                controller(
                'SilversolutionsEshopBundle:Navigation:showTopNavigation',
                {'locationId' : 2, 'depth' : 3}
                ),
                {}
                ) }}
            </section>
            {% block header_feature_nav %}
              <div class="c-icon-bar__wrapper js-icon-bar">
                <div class="icon-bar three-up">
                  <div class="item with-border hide-for-large-up">
                    <a href="{{ path('silversolutions_search') }}">
                      <i class="fa fa-search"></i>
                      <label>{{ 'Search'|st_translate }}</label>
                    </a>

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
          </nav>

{% endblock %}
```
