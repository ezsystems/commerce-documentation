# Component - Print

Print specific styles in addition to Foundation built in styling.

### What to we extend / override:

- hide href attribute value after each link

## Sass:

**File location**

``` 
scss/storm/_components.print.scss
```

### Important note:

Make sure to import the component as last file in: vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/style.scss in order to override styles without \!important

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/style.scss**

``` 
// print goes last as in order to override styles without important
@import "storm/_components.print.scss";
```

## Foundation settings:

In order to enable / disable Foundation print styles please use the following variable:

``` 
// vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/settings/_foundation.scss
// true - enabled
// false - disabled
...
$include-print-styles: true;
```

## Useful classes

| Class          | Description                |
| -------------- | -------------------------- |
| large-12@print | Full width for print media |

### HTML Example

``` 
<div class="large-9 large-12@print columns">
                <h1 class="left">{{ catalogElement.name }}</h1>
                {% block childrenList %}
                    {% render(controller('SisoSearchBundle:Search:productList', {
                    'locationId': catalogElement.identifier,
                    })) %}
                {% endblock %}
            
```

On desktop it takes 9 columns, but for print it's full width. 
