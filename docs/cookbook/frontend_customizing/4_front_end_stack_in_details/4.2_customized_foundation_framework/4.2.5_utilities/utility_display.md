# Utility - Display

We use this file to create display block/inline/inline-block classes for different breakpoints.

It is actually a Sass mixin which generates classes.

## Sass

``` 
scss/storm/_utils.display.scss
```

### Available classes

#### Every screen size:

.u-inline  
.u-block  
.u-inline-block

#### Small screen size

.u-inline-on-small  
.u-block-on-small  
.u-inline-block-on-small

#### Medium screen size

.u-inline-on-medium  
.u-block-on-medium  
.u-inline-block-on-medium

#### Large screen size

.u-inline-on-large  
.u-block-on-large  
.u-inline-block-on-large

You can combine those classes within one element or just use them separately. 

## HTML

### Make sure element is displayed as inlined

``` 
<div class="u-inline">
...

```

Even though `<div>` is a block level element you can force with this utility class to be displayed as inlined.

### Display differently based on screen size

``` 
<div class="u-inline u-block-on-small">
...

```

Using this approach the element will be treated like an inline on every screen size except small range.
