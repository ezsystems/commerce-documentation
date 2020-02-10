#  Component - Autocomplete 

It's a Sass file for an Ajax Autocomplete for jQuery plugin (<https://github.com/devbridge/jQuery-Autocomplete>).

## Sass

**File location**

``` 
scss/storm/_components.autocomplete.scss
```

### Default settings:

``` 
$autocomplete-border-width: 1px;
$autocomplete-border-style: solid;
$autocomplete-border-color: $aluminum;

$autocomplete-bg-color: $white;

$autocomplete-box-offset-x: 1px;
$autocomplete-box-offset-y: 4px;
$autocomplete-box-blur: 3px;
$autocomplete-box-spread: 0;
$autocomplete-box-color: $iron;

$autocomplete-highlight-font-weight: bold;
$autocomplete-highlight-font-color: $black;

$autocomplete-item-padding: rem-calc(2px 5px);
$autocomplete-selected-bg-color: $snow;

$autocomplete-group-title-font-weight: bold;
$autocomplete-group-title-font-size: rem-calc(16);
$autocomplete-group-title-font-color: $black;
$autocomplete-group-title-border-bottom-width: 1px;
$autocomplete-group-title-border-bottom-style: solid;
$autocomplete-group-title-border-bottom-color: black;
```

We don't use BEM for this component. We don't want to override any classes that might be hardcoded in the jQuery Plugin

In order to change settings in project find settings/\_storm.scss file in your project and find the Autocomplete section.
