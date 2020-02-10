#  Component - Search 

Component used for the search field in the header area

## Sass

**File location**

``` 
scss/storm/_components.search.scss
```

### Default settings:

``` 
$search-input-border-width: rem-calc(1);
$search-input-border-style: solid;
$search-input-boder-color: $secondary-color;
$search-input-height: rem-calc(40);

$search-button-background: lighten($secondary-color, 10%);
$search-button-color: $primary-color;
$search-button-border-width: rem-calc(1);
$search-button-border-style: solid;
$search-button-boder-color: $secondary-color;
$search-button-height: rem-calc(40);
$search-button-hover-background: $secondary-color;
$search-button-hover-color: $primary-color;

$search-dropdown-background: darken($primary-color, 5%);
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Search section.

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig**

``` 
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

```
