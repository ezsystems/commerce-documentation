# Extend - Buttons

Extends: http://foundation.zurb.com/sites/docs/v/5.5.3/components/buttons.html

## What/why do we extend?

1. We needed a dimmed version of a button secondary type

## Sass

**File location**

``` 
scss/storm/_extend.components.buttons.scss
```

### Default settings:

``` 
$button-success-color: $jet;
$button-secondary-dimmed-background: lighten($secondary-color, 30%);
$button-secondary-dimmed-background-hover: $secondary-color;
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Buttons section.

## HTML

``` 
<button class="button secondory dimmed">Dimmed button</button>
 
<a class="button secondory dimmed" href="#">Dimmed button</a>
```
