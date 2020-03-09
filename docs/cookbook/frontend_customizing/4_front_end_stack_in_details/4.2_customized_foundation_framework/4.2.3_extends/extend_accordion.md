# Extend - Accordion

Extends: <http://foundation.zurb.com/sites/docs/v/5.5.3/components/accordion.html>

## What/why do we extend?

1. We want to have an icon that indicated that tab is opened/closed.
1. Custom styling for tables displayed inside tab content.
1. Manipulate spacing (size) of a tab.

## Sass

**File location**

``` 
scss/storm/_extend.components.accordion.scss 
```

### Default settings

``` 
$accordion-nav-border-bottom-width: 1px;
$accordion-nav-border-bottom-style: solide;
$accordion-nav-border-bottom-color: $white;
$accordion-nav-link-position: relative;
$accordion-nav-link-icon-position: absolute;
$accordion-nav-link-icon-right: ($column-gutter / 2);
$accordion-nav-link-icon-symbol: '?';
$accordion-nav-link-active-icon-symbol: '?';
$accordion-nav-link-icon-color: $flat-wet-asphalt;
$accordion-nav-link-icon-font-family: FontAwesome;
$accordion-nav-small-link-padding: ($accordion-navigation-padding / 2);

$accordion-content-inner-table-border-color: $accordion-navigation-active-bg-color;
$accordion-content-inner-table-border-top: none;

$accordion-content-bordered-border-width: $accordion-border-width;
$accordion-content-bordered-border-style: solid;
$accordion-content-bordered-border-width: $accordion-navigation-active-bg-color;
$accordion-content-inner-table-border-top-width: 0;
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Accordion section.

### How to make smaller accordion title?

``` 
<ul class="accordion js-search-facets-accordion" data-accordion data-options="multi_expand: true">
    <li class="accordion-navigation small">
        <a href="#panel-{{ facetGroup.labelKey|lower }}">{{ facetGroup.labelKey|st_translate('search') }}</a>
        <div id="panel-{{ facetGroup.labelKey|lower }}" class="content content--bordered{% if loop.index == 1 %} active{% endif %} u-padding-1x">
        ...
        
    </li>
</ul>
```

**Notice**: *accordion-navigation small* applied to an `<li>` element.
