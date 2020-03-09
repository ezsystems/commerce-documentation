# Extended reveal modal

In order to make it easy to use we have created and abstraction layer on top of reveal modal from Foundation (<http://foundation.zurb.com/sites/docs/v/5.5.3/components/reveal.html>). 

## JavaScript:

**File location**

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/phalanx/storm.phalanx.utils.js
```

Phalanx utility file contains couple of helpers. Reveal is one of them.

### Available methods:

#### storm.reveal.open(id, content, type, size)

Opens the modal window using Foundation Reveal Modal. It takes four parameters:

- id - modal window ID (must be unique
- content - content that is displayed inside a modal
- type - type of modal window e.g: alert, success, info, warning
- size - size of the modal window (tiny, small, medium, large, xlarge, full). Default is medium

Base example:

```
var content = 'html content';
storm.reveal.open('my-modal-window', content, '');
```

It will open standard modal window, medium sized.

Large modal window:

```
var content = 'html content';
storm.reveal.open('my-modal-window', content, '', 'large');
```

#### storm.reveal.destroy(id)

Destroys a modal window. I takes one parameter:

- id - modal window ID

```
storm.reveal.destroy('my-modal-window');
```

## Sass:

**File location**

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/storm/_extend.components.reveal.scss
```

We extend the default Foundation component by adding some extra styling on top of it. With our extended version it's possible to make the modal window look like alert message (with coloured background for success, error, info or warning).

If you want to change/extend it please copy it to your project location.
