#  Extend - Label 

Extends: <http://foundation.zurb.com/sites/docs/v/5.5.3/components/labels.html>

## What/why do we extend?

1.  To style content generated by WYSIWYG.
2.  To extend a bit the functionality offered by the base.

## Sass

**File location**

``` 
scss/storm/_components.extend.label.scss
```

### Default settings:

``` 
$label-paragraph-display: inline;
$label-paragraph-font-size: rem-calc(11);

$label-info-link-color: $white;
$label-info-link-font-size: rem-calc(11);
$label-info-link-text-decoration: underline;
$label-info-link-hover-color: $white;
$label-info-link-hover-text-decoration: none;

$label-warning-color: darken($alert-warning-border-color, $alert-text-color-darken);
$label-secondary-color: darken($alert-secondary-border-color, $alert-text-color-darken);
$label-success-color: darken($alert-success-border-color, $alert-text-color-darken);
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Label section.

## HTML

``` 
<span class="label">
    <p>Lorem ipsum dolor sit amet.</p>

```

``` 
<span class="label info">
    <p>Lorem ipsum <a href="#">dolor sit</a> amet.</p>

```