#  Extend - Forms 

Extends: <http://foundation.zurb.com/sites/docs/v/5.5.3/components/forms.html>

## What/why do we extend?

1.  In order to solve long labels problem we have introduced an extension to Foundation forms. It's useful for checkboxes and radio button with long labels.
2.  In order to remove spacing for \<p\> tags inside \<label\>s.

## Sass

**File location**

``` 
scss/storm/_components.extend.forms.scss
```

### Default settings:

``` 
$pair-wrapper-label-padding-left: rem-calc(20);
$pair-wrapper-paragraph-padding-right: 0;
$pair-wrapper-paragraph-inner-color: $form-label-font-color;
$pair-wrapper-paragraph-inner-font-weight: normal;
$pair-wrapper-required-symbol: '*';

$form-label-paragraph-inner-display: inline-block;
$form-label-paragraph-inner-margin-bottom: 0;
$form-label-paragraph-inner-padding-right: rem-calc(5);
$form-label-paragraph-inner-font-size: rem-calc(14);
```

In order to change settings in project find settings/\_storm.scss file in your project and find the Forms section.

## HTML

### Basic example

``` 
<div class="c-pair-wrapper">
  <input type="checkbox" id="terms-and-conditions">
  <label for="terms-and-conditions">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere laudantium ex repellendus quia vero debitis libero. Ea incidunt quis quasi velit quibusdam minus eveniet, fugiat quos inventore, alias animi aperiam!</label>

```

### Add an asterisk at the end of label for required field

``` 
<div class="c-pair-wrapper c-pair-wrapper--required">
  <input type="checkbox" id="terms-and-conditions">
  <label for="terms-and-conditions">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere laudantium ex repellendus quia vero debitis libero. Ea incidunt quis quasi velit quibusdam minus eveniet, fugiat quos inventore, alias animi aperiam!</label>

```

### Add an asterisk at the end of label for required field and label translation/description comes from an online editor.

``` 
<div class="c-pair-wrapper c-pair-wrapper--required c-pair-wrapper__text-module">
  <input type="checkbox" id="terms-and-conditions">
  <label for="terms-and-conditions">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere laudantium ex repellendus quia vero debitis libero. Ea incidunt quis quasi velit quibusdam minus eveniet, fugiat quos inventore, alias animi aperiam!</label>

```

## Twig

**Twig - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/register\_private.html.twig**

``` 
...
<div class="js-ajax-get-content{% if form.termsAndConditions.vars.errors is not empty %} error{% endif %}"
     data-content-selector=".js-ez-article">
  <div class="c-pair-wrapper">
    {{ form_widget(form.termsAndConditions) }}
    {% include 'SilversolutionsEshopBundle:Forms:custom_form_label.html.twig'|st_resolve_template
    with {
    'form_row': form.termsAndConditions
    } %}
  
  {% for error in form.termsAndConditions.vars.errors %}
    <small class="error">{{ error.message }}</small>
  {% endfor %}

...
```
