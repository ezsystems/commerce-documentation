#  Utility - Spacing 

We use this class to manipulate margin and padding on elements.

## Sass

**File location**

``` 
scss/storm/_utils.spacing.scss
```

### Available classes

#### Every screen size

/\* margin \*/  
.u-no-margin  
.u-no-margin-top  
.u-no-margin-right  
.u-no-margin-bottom  
.u-no-margin-left

.u-margin-1x  
.u-margin-2x  
.u-margin-3x  
.u-margin-4x

.u-margin-top-1x  
.u-margin-top-2x  
.u-margin-top-3x  
.u-margin-top-4x

.u-margin-right-1x  
.u-margin-right-2x  
.u-margin-right-3x  
.u-margin-right-4x

.u-margin-bottom-1x  
.u-margin-bottom-2x  
.u-margin-bottom-3x  
.u-margin-bottom-4x

.u-margin-left-1x  
.u-margin-left-2x  
.u-margin-left-3x  
.u-margin-left-4x

/\* padding \*/

.u-no-padding  
.u-no-padding-top  
.u-no-padding-right  
.u-no-padding-bottom  
.u-no-padding-left

.u-padding-1x  
.u-padding-2x  
.u-padding-3x  
.u-padding-4x

.u-padding-top-1x  
.u-padding-top-2x  
.u-padding-top-3x  
.u-padding-top-4x

.u-padding-right-1x  
.u-padding-right-2x  
.u-padding-right-3x  
.u-padding-right-4x

.u-padding-bottom-1x  
.u-padding-bottom-2x  
.u-padding-bottom-3x  
.u-padding-bottom-4x

.u-padding-left-1x  
.u-padding-left-2x  
.u-padding-left-3x  
.u-padding-left-4x

####  

####  Small screen size

.u-no-margin-on-small  
.u-no-margin-top-on-small  
.u-no-margin-right-on-small  
.u-no-margin-bottom-on-small  
.u-no-margin-left-on-small  
.u-margin-1x-on-small  
.u-margin-top-1x-on-small  
.u-margin-bottom-1x-on-small  
.u-margin-left-1x-on-small  
.u-padding-1x-on-small

####  Medium screen size

.u-no-margin-on-medium  
.u-no-margin-top-on-medium  
.u-no-margin-right-on-medium  
.u-no-margin-bottom-on-medium  
.u-no-margin-left-on-medium  
.u-margin-1x-on-medium  
.u-margin-top-1x-on-medium  
.u-margin-bottom-1x-on-medium  
.u-margin-left-1x-on-medium  
.u-padding-1x-on-medium  
.u-no-margin-on-medium-down  
.u-no-padding-on-medium-down  
.u-no-margin-on-medium-up  
.u-no-padding-on-medium-up

####  Large screen size

.u-no-margin-on-large  
.u-no-margin-top-on-large  
.u-no-margin-right-on-large  
.u-no-margin-bottom-on-large  
.u-no-margin-left-on-large  
.u-margin-1x-on-large  
.u-margin-top-1x-on-large  
.u-margin-bottom-1x-on-large  
.u-margin-left-1x-on-large  
.u-padding-1x-on-large  
.u-no-margin-on-large-up  
.u-no-margin-bottom-on-large-up  
.u-no-padding-on-large-up

#### On every device smaller than large

.u-no-margin-on-large-down  
.u-no-padding-on-large-down

We provide 4 levels of spacing for margin and padding. Base space is set to half size of the column gutter (15px). Following this rule \*-2x means 30px, \*-3x means 45px and \*-4x means 60px of margin or padding.

## HTML

**Different margin on different screen sizes**

``` 
<div class="u-margin-2x u-margin-1x-on-large u-no-margin-left-on-small">
...

```

As you can see it's easy to combine classes just like you want.
