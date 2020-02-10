#  Component - Spinner 

We use spinner component to show that some actions are working in the background with a nice animation. We use Font Awesome icon font for the icon and animation.

## Sass

**File location**

``` 
scss/storm/_components.spinner.scss
```

### Default settings:

``` 
$spinner-position: fixed;
$spinner-zindex: 9998;
$spinner-bg-color: rgba(0, 0, 0, 0.45);

$spinner-icon-display: inline-block;
$spinner-icon-margin-left: rem-calc(-24);
$spinner-icon-margin-top: rem-calc(-36);
$spinner-icon-font-size: rem-calc(48);
$spinner-icon-font-family: FontAwesome, serif;
$spinner-icon-color: $white;
$spinner-icon-symbol: "\f110";

$spinner-inner-icon-font-size: rem-calc(32);
$spinner-inner-icon-color: $primary-color;

$spinner-contained-zindex: 98;
$spinner-contained-icon-color: $primary-color;
$spinner-contained-bg-color: rgba(255, 255, 255, 0.45);
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Spinner section.

## HTML

Spinner component offers 3 different possibilities for displaying a spinner:

  - full page which covers the whole page
  - inner which is displayed inside an element
  - contained which covers only a section of page

### Examples

#### Full page

In order to show full page spinner append this snippet as a direct children of \<body\> tag. We use this type of spinner especially for Ajax action types.

``` 
...
<body>
....
<div class="spinner">
...
</body>
....
```

#### Inner 

``` 
...
<body>
....
<aside>
    <div class="spinner inner">
</aside>
...
</body>
....
```

#### Contained

``` 
...
<body>
....
<section>
<h3>Section title</h3>
<div class="spinner contained">
</section>
...
</body>
....
```

## JavaScript

As mentioned before we use full page spinner for Ajax actions to show to the user that something is happening in the background. We have prepared 2 functions in order to show/hide a spinner. 

``` 
storm.spinner.show();
storm.spinner.hide();
```

These functions are located in: js/phalanx/storm.phalanx.utils.js. 
