# Scroll to top

We use this component to help user go back to the top of the page. 

## Files

``` 
// Sass
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/storm/_components.scroll.scss
 
// JavaScript
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/components/scroll.js
```

### HTML snippet

In order to make it work you also need to include an HTML. We use pagelayout in order to make use of block overriding in projects. 

``` 
{% block scrollToTop %}
  <a class="c-scroll hide js-scroll-to-top" href="#" data-offset="200">
    <i class="fa fa-arrow-circle-up fa-3x"></i>
  </a>
{% endblock %}
```

## Configuration

We use data-\* attributes to make it easy to configure

| Setting     | Description                                                                                                                    |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------ |
| data-offset | Define the offset from the top of page. When the offset is reached we show/hide scroll top arrow with fadeIn/fadeOut animation |

**Additionally the wrapper needs these CSS classes to make it work**

| Class            | Description                                                                                          |
| ---------------- | ---------------------------------------------------------------------------------------------------- |
| hide             | Hides scroll to top by default. This class is part of Foundation standard.                           |
| js-scroll-to-top | JavaScript hook class. Using this class JavaScript knows which part of HTML we should interact with. |

## Examples

### Override in a project

1.  Put your code inside a {% block scrollToTop %}{% endblock %}
2.  Append class to hide it by default e.g. hide
3.  Appen js-srcoll-to-top class to connect it with JS

### Offset change

In case you wish to change when scroll to top show/hide you need to change the data-offset setting:

``` 
{% block scrollToTop %}
  <a class="c-scroll hide js-scroll-to-top" href="#" data-offset="250">
    <i class="fa fa-arrow-circle-up fa-3x"></i>
  </a>
{% endblock %}
```

It will show when you scroll more than 250 pixels from top. It will hide when you reach less than 250 pixels.

### Override JavaScript

There is a global object available in the JavaScript scope if you wish to override it. It's called window.storm.scroll. The object has one method called init. For details please take a deeper look at the source code of this file

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/components/scroll.js
```
