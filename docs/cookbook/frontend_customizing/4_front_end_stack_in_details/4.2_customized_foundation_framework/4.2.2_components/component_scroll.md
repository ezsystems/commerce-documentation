#  Component - Scroll 

## Sass

**File location**

``` 
scss/storm/_components.scroll.scss
```

### Default settings:

``` 
$scroll-to-top-position-right: ($column-gutter / 2);
$scroll-to-top-position-bottom: rem-calc(62);

$scroll-to-top-color: $flat-peter-river;

$scroll-to-top-size: rem-calc(20);
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Autocomplete section.

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig**

``` 
<a class="c-scroll hide hide-for-print js-scroll-to-top" href="#" data-offset="200">
  <span class="fa-stack fa-2x">
    <i class="fa fa-circle fa-stack-2x"></i>
    <i class="fa fa-arrow-up fa-stack-1x fa-inverse"></i>
  
</a>
```
