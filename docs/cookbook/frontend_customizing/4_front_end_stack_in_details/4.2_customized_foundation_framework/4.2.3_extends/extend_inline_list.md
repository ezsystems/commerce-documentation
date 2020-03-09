# Extend - Inline List

Extends: <http://foundation.zurb.com/sites/docs/v/5.5.3/components/inline_lists.html>

## What/why do we extend?

1. In order to customise the way how it looks.

## Sass:

``` 
scss/storm/_extend.components.inline-lists.scss
```

### Default settings:

``` 
$inline-list-font-color: $primary-color;
$inline-list-parent-font-size: rem-calc(12); // first level if there is a dropdown inside inline list
$inline-list-children-font-size: rem-calc(14); // general font size for link items inside inline lists
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Inline Lists section.
