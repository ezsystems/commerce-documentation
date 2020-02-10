#  Utility - Float 

We use this utility to extend what Foundation offers in terms of floats. 

## Sass

**File location**

``` 
scss/storm/_utils.float.scss
```

### Available classes

#### Every screen size

.u-no-float

####  Small screen size

.u-no-float-on-small  
.u-float-right-on-small  
.u-float-left-on-small

####  Medium screen size

.u-no-float-on-medium  
.u-float-right-on-medium  
.u-float-left-on-medium

####  Large screen size

.u-no-float-on-large  
.u-float-right-on-large  
.u-float-left-on-large

## HTML

**Remove floats set by another classes**

``` 
<div class="right u-no-float-on-small">
...

```

In that case we use **"right" ** class which ships with Foundation. Using our utility we are removing floats on small devices.
