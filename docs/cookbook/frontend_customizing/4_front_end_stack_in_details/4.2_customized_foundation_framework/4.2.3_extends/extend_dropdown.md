#  Extend - Dropdown 

Extends: <http://foundation.zurb.com/sites/docs/v/5.5.3/components/dropdown.html>

## What/why do we extend?

1.  We needed in between size for a dropdown (between tiny/small)

## Sass

**File location**

``` 
scss/storm/_extend.components.dropdown.scss
```

### Default settings:

``` 
$dropdown-smaller-max-width: rem-calc(250);

$dropdown-flexible-width: 95% !important;
$dropdown-flexible-left: 2.5% !important;
$dropdown-flexible-medium-only-right: ($column-gutter/2) !important;
$dropdown-flexible-medium-only-left: auto !important;
$dropdown-flexible-medium-only-tiny-max-width: rem-calc(200) !important;
$dropdown-flexible-medium-only-small-max-width: rem-calc(300) !important;
$dropdown-flexible-medium-only-medium-max-width: rem-calc(500) !important;
$dropdown-flexible-large-up-width: auto !important;
$dropdown-flexible-large-up-tiny-max-width: rem-calc(200) !important;
$dropdown-flexible-large-up-small-max-width: rem-calc(300) !important;
$dropdown-flexible-large-up-medium-max-width: rem-calc(500) !important;
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Dropdown section.

## HTML

``` 
<a data-dropdown="drop1" aria-controls="drop1" aria-expanded="false">Has Dropdown</a>
<ul id="drop1" class="f-dropdown smaller" data-dropdown-content aria-hidden="true" tabindex="-1">
  <li><a href="#">This is a link</a></li>
  <li><a href="#">This is another</a></li>
  <li><a href="#">Yet another</a></li>
</ul>
```

## Modifier classes

We have introduced couple of modifier classes

| CSS class | Description                                                                                              |
| --------- | -------------------------------------------------------------------------------------------------------- |
| no-arrow  | Use this class to remove a CSS arrow inside a dropdown content.                                          |
| flexible  | In order to make it work on every device and every place we extended .f-dropdown with a .flexible class. |

### Examples

#### Remove arrow from dropdown content: 

``` 
// Example from icon bar
  <a class="c-icon-bar__arrow" data-dropdown="dropdown-basket">
    <label class="show-for-large-up">{{ 'Shopping basket'|st_translate }}</label>
  </a>
  <div id="dropdown-basket" class="f-dropdown medium content text-left no-arrow" data-dropdown-content>
  ...
  
```

#### Remove arrow and make dropdown content flexible to work on every device:

``` 
// Example from icon bar
  <a class="c-icon-bar__arrow" data-dropdown="dropdown-basket">
    <label class="show-for-large-up">{{ 'Shopping basket'|st_translate }}</label>
  </a>
  <div id="dropdown-basket" class="f-dropdown medium content text-left no-arrow flexible" data-dropdown-content>
  ...
  
```
