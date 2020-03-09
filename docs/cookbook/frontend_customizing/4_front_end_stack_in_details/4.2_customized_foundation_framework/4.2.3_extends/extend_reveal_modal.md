# Extend - Reveal Modal

Extends: http://foundation.zurb.com/sites/docs/v/5.5.3/components/reveal.html

## What/why do we extend?

1. We had to extend in order to handle some custom situations like Ajax response messages, etc.

## Sass:

**File location**

``` 
scss/storm/_components.extend.reveal.scss
```

### Default settings:

``` 
$reveal-error-color: $alert-color;
$reveal-error-close-color: $white;

$reveal-success-color: $success-color;
$reveal-success-close-color: $white;

$reveal-info-color: $info-color;
$reveal-info-close-color: $white;

$reveal-with-text-position: fixed;
$reveal-with-text-top: 5% !important;
$reveal-with-text-left: 50%;
$reveal-with-text-max-height: 0;
$reveal-with-text-min-height: 90%;
$reveal-with-text-width: 90%;
$reveal-with-text-overflow-y: auto;
$reveal-with-text-transform: translateX(-50%);

$reveal-no-full-screen-left: 50%;
$reveal-no-full-screen-padding: rem-calc(16);
$reveal-no-full-screen-max-height: 0;
$reveal-no-full-screen-min-height: 80vh;
$reveal-no-full-screen-height: auto;
$reveal-no-full-screen-width: 90%;
$reveal-no-full-screen-transform: translateX(-50%);
```

In order to change settings in project find `settings/_storm.scss` file in your project and find the Reveal section.

## JavaScript

In order to make our life easier we have created some helper functions to handle dynamically content inside reveal modal.

These functions are located in our storm utils file:

``` 
js/phalanx/storm.phalanx.utils.js
```

### Available functions:

#### storm.reveal.open()

**storm.reveal.open()**

``` 
storm.reveal.open(id, content, type, size);
```

As you can see this function takes 4 parameters:

|Parameter|Description|
|--- |--- |
|id|modal window id e.g. ajax-response-modal|
|content|html/text content that should be displayed inside a modal window|
|type|Type of message, can be empty.</br>Possible values: success, error</br>If empty the window will have standard white background.|
|size|Size of the window.</br>Possible values: tiny, small, medium, large, xlarge, full</br>default: medium|

#### storm.reveal.destory();

``` 
storm.reveal.destroy(id);
```

This function takes only one parameter which is modal window id that should be destroyed.

### Examples

#### Open a modal window

``` 
var content = '<h1>Hello world</h1><p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Hic aspernatur eaque aperiam. Perferendis veritatis facilis vel ad possimus, voluptatem dolore. Ea debitis id voluptatibus fugiat esse laborum excepturi consectetur. Sed.</p>';

storm.reveal.open('custom-modal', content, '', 'small');
```

*Open a **small** modal window with some basic content. No type specified so it will be just standard white background color.*

#### Close a modal window

In order to destroy created modal window call destroy function like this:

``` 
storm.reveal.destroy('custom-modal');
```
