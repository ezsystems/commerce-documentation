#  Extend - Panels 

Extends: <http://foundation.zurb.com/sites/docs/v/5.5.3/components/panels.html>

## What/why do we extend?

1.  We needed a panel with smaller padding around the panel's content

## Sass

**File location**

``` 
scss/storm/_extend.components.panels.scss
```

### Default settings:

``` 
$panel-small-padding: ($panel-padding / 2);
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Panels section.

## Example

``` 
<div class="panel small">
  <h5>This is a regular panel.</h5>
  <p>It has an easy to override visual style, and is appropriately subdued.</p>

```
