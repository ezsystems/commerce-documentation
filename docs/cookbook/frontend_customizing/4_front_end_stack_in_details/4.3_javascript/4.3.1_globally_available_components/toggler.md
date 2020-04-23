# Toggler

We use toggler component to toggle content visibility. It's a well know pattern on many websites and online shops. 

## Sass

In order to find out more about Sass part of this component please visit: [Component - Toggler](../../4.2_customized_foundation_framework/4.2.2_components/component_toggler.md)

## JavaScript

Toggler component is part of our main JavaScript file: app.js 

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/app.js
```

We use `data-*` attributes to configure toggler:

| Attribute      | Description                                                  |
| -------------- | ------------------------------------------------------------ |
| data-class     | CSS class that is going to be toggled e.g. hidden, hide, etc |
| data-target    | toggled item                                                 |
| data-parent    | toggled items wrapper                                        |
| data-text-swap | toggled text inside a link                                   |

### Example

``` 
<div class="c-toggler">
  <a href="#" class="c-toggler__trigger js-toggler"
  data-class="hidden"
  data-target="js-toggler-item"
  data-parent="js-toggler-wrapper"
  data-text-swap="Show lesss">Show more</a>

<div class="js-toggler-wrapper">
  <div class="js-toggler-item">Content goes here
  <div class="js-toggler-item">Content goes here
  <div class="js-toggler-item">Content goes here
  <div class="js-toggler-item hidden">Content goes here
  <div class="js-toggler-item hidden">Content goes here
  <div class="js-toggler-item hidden">Content goes here

```
