#  st\_tag - selector tagging for Behat and Google Tag manager 

# Objective

Template operator st\_tag creates custom html attribute, that allows to quickly find html tag with this attribute.

This is especially helpful for Behat testing, because st\_tag provides consistent naming convention for page selectors.

### Example

Twig template

``` 
<p {{ st_tag('product', 'price', 'read') }}>1.865,00 €</p>
```

is rendered to html:

``` 
<p data-tag-product-price-read>1.865,00 €</p>
```

# How to enable tagging

To enable tagging define a dynamic site access aware parameter siso\_tools.default.tag-prefix

**Example**

``` 
siso_tools.default.tag-prefix: data-tag
```

If you set an empty value to this parameter, then all tags will be hidden. It is important, for example, in prod mode to get rid of behat tags.

# Implementation

Taaging is implemented with a template operator st\_tag.

The operator should take a few parameters:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>Module/Component</td>
<td><p><em>required (string)</em></p>
<p>Identifies module/component/page that we are working on.</p></td>
<td>product, form</td>
</tr>
<tr>
<td>Element (target)</td>
<td><p><em>required (string)</em></p>
<p>Identifies specific field which we are searching for.</p></td>
<td>price, field, text, label</td>
</tr>
<tr>
<td>Action</td>
<td><p><em>required (string)</em></p>
<p>Identifies desired action for the selector.</p></td>
<td><ul>
<li>read - reads to value of text node</li>
<li>fill/write - fill in form field</li>
<li>click/follow/submit - clicking on buttons, following links</li>
</ul></td>
</tr>
<tr>
<td>ID</td>
<td><p><em>optional (number, string)</em></p>
<p>Identifies unique element that occurs more than once on a page. </p>
<p>Helpful when searching for specific element in collections, e.g. product list, multiple images, buttons, etc.</p></td>
<td><p>1200, button_action</p></td>
</tr>
<tr>
<td>Value</td>
<td><p><em>optional (mix)</em></p>
<p>If we don't want to take the value from the wrapper because it contains some special characters or format we can use this parameter to set a standard format.</p>
<p>Could be helpful to get price for example.</p></td>
<td>1600.00 (instead of 1.600,00 €)</td>
</tr>
</tbody>
</table>

Don't use these attributes in CSS (for styling) or JavaScript (for selecting, hooks, etc). These attributes have completely different and specific purpose. In addition in production mode can be disabled and than you selectors will be broken.

# Examples

**1. Read price from**

``` 
<p {{ st_tag('product', 'price', 'read') }}>1.865,00 €</p>
```

Output:

``` 
<p data-tag-product-price-read>1.865,00 €</p>
```

**2. Read price with custom value**

``` 
<p {{ st_tag('product', 'price', 'read', '', '1865.00') }}>1.865,00 €</p>
```

Output:

``` 
<p data-tag-product-price-read="1865.00">1.865,00 €</p>
```

**3. Follow/click on a text link**

``` 
<a href="/link-to-product.html" {{ st_tag('product', 'anchor', 'follow', '1200', '') }}>Paprika3</p>
```

Output:

``` 
<a href="/link-to-product.html" data-tag-product-anchor-follow-1200>Paprika3</p>
```

**4. Filling out form field** 

``` 
<input name="quantity" id="quantity" value="" {{ st_tag('form', 'input', 'write', 'quantity', '') }}>
```

Output:

``` 
<input name="quantity" id="quantity" value="" data-tag-form-input-write-quantity>
```

**5. Checks if there is a specific message on the page (e.g. after the ajax response)**

``` 
<div class="message notice" {{ st_tag('message', 'notice', 'read', '', '') }}>Message content
```

Output:

``` 
<div class="message notice" data-tag-message-notice-read>Message content
```

**6. Submit Buttons or links**

**Important note:**

Please use always the same naming for buttons submitting data. The reason is that e.g. a Behat test or google tag manager don't care if it is a button, a link or an input field which will send the form\!

``` 
<button name="add_to_basket" type="submit" value="Kaufen" class="button add_to_basket float_right"> {{ st_tag('product', 'order', 'submit', '', '') }} 
```

Output:

``` 
<input name="quantity" id="quantity" value="" data-tag-product-order-submit>
```
