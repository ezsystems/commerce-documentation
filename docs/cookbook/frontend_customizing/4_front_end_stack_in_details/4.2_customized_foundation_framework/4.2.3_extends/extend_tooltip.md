#  Extend - Tooltip 

<http://foundation.zurb.com/sites/docs/v/5.5.3/components/tooltips.html>

## What/why do we extend?

1.  We want that the tooltip nub looks good on mobile

## Sass

**File location**

``` 
scss/storm/_extend.components.tooltips.scss
```

## Twig/HTML

In order to make it work on mobile devices when you wrap the tooltip with an \<a\> element make sure the tooltip resides in a separate span

``` 
<li class="c-text-buttons__item is-disabled">
  <a href="#">
    <span class="u-block u-text-small tip-left" data-tooltip aria-haspopup="true" title="{{ 'Please log in to use this feature'|st_translate }}">
      <i class="fa fa-heart fa-lg fa-fw"></i> {{ 'Add to wishlist'|st_translate }}
    
  </a>
</li>
```
