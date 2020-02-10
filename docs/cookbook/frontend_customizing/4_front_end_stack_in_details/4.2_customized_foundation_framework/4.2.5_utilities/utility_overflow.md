#  Utility - Overflow 

We use this utility to set the overflow property. It was created to make it easy to override some behaviour of components from Foundation.

## Sass

``` 
scss/storm/_utils.overflow.scss
```

### Available classes

#### Every screen size

.u-overflow-auto

####  Small screen size

.u-overflow-auto-on-small

####  Medium screen size

.u-overflow-auto-on-medium

####  Large screen size

.u-overflow-auto-on-large

## HTML

``` 
<div class="... u-overflow-auto-on-large">
...

```

It sets overflow: auto; for large screen devices. For every other screen it will apply overflow settings from the other classes.
