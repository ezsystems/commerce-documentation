# `st_tag` - selector tagging for Behat and Google Tag manager

## Objective

Template operator st\_tag creates custom html attribute, that allows to quickly find html tag with this attribute.

This is especially helpful for Behat testing, because st\_tag provides consistent naming convention for page selectors.

### Example

Twig template

``` html+twig
<p {{ st_tag('product', 'price', 'read') }}>1.865,00 €</p>
```

is rendered to html:

``` html+twig
<p data-tag-product-price-read>1.865,00 €</p>
```

## How to enable tagging

To enable tagging define a dynamic site access aware parameter siso\_tools.default.tag-prefix

**Example**

``` 
siso_tools.default.tag-prefix: data-tag
```

If you set an empty value to this parameter, then all tags will be hidden. It is important, for example, in prod mode to get rid of behat tags.

## Implementation

Taaging is implemented with a template operator st\_tag.

The operator should take a few parameters:

|Parameter|Description|Example|
|--- |--- |--- |
|Module/Component|required (string)</br>Identifies module/component/page that we are working on.|product, form|
|Element (target)|required (string)</br>Identifies specific field which we are searching for.|price, field, text, label|
|Action|required (string)</br>Identifies desired action for the selector.|read - reads to value of text node</br>fill/write - fill in form field</br>click/follow/submit - clicking on buttons, following links|
|ID|optional (number, string)</br>Identifies unique element that occurs more than once on a page. </br>Helpful when searching for specific element in collections, e.g. product list, multiple images, buttons, etc.|1200, button_action|
|Value|optional (mix)</br>If we don't want to take the value from the wrapper because it contains some special characters or format we can use this parameter to set a standard format.</br>Could be helpful to get price for example.|1600.00 (instead of 1.600,00 €)|

!!! caution

    Don't use these attributes in CSS (for styling) or JavaScript (for selecting, hooks, etc). These attributes have completely different and specific purpose. In addition in production mode can be disabled and than you selectors will be broken.

## Examples

### Read price from

``` html+twig
<p {{ st_tag('product', 'price', 'read') }}>1.865,00 €</p>
```

Output:

``` html+twig
<p data-tag-product-price-read>1.865,00 €</p>
```

### Read price with custom value

``` html+twig
<p {{ st_tag('product', 'price', 'read', '', '1865.00') }}>1.865,00 €</p>
```

Output:

``` html+twig
<p data-tag-product-price-read="1865.00">1.865,00 €</p>
```

### Follow/click on a text link

``` html+twig
<a href="/link-to-product.html" {{ st_tag('product', 'anchor', 'follow', '1200', '') }}>Paprika3</p>
```

Output:

``` html+twig
<a href="/link-to-product.html" data-tag-product-anchor-follow-1200>Paprika3</p>
```

### Filling out form field

``` html+twig
<input name="quantity" id="quantity" value="" {{ st_tag('form', 'input', 'write', 'quantity', '') }}>
```

Output:

``` html+twig
<input name="quantity" id="quantity" value="" data-tag-form-input-write-quantity>
```

### Checks if there is a specific message on the page (e.g. after the ajax response)

``` html+twig
<div class="message notice" {{ st_tag('message', 'notice', 'read', '', '') }}>Message content
```

Output:

``` html+twig
<div class="message notice" data-tag-message-notice-read>Message content
```

### Submit Buttons or links

!!! caution

    Please use always the same naming for buttons submitting data. The reason is that e.g. a Behat test or google tag manager don't care if it is a button, a link or an input field which will send the form\!

``` xml
<button name="add_to_basket" type="submit" value="Kaufen" class="button add_to_basket float_right"> {{ st_tag('product', 'order', 'submit', '', '') }} 
```

Output:

``` xml
<input name="quantity" id="quantity" value="" data-tag-product-order-submit>
```
