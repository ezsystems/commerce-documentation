#  Basket - add products to the basket 

### Which data is necessary?

There are some fixed parameters, that are necessary to be passed, if you want to sucessfully add products to the basket. Still, the parameters are handled in a flexible way, so any other parameter, that is set in the correct way, will be passed and stored to the basket line.

<table>
<thead>
<tr class="header">
<th><p>Fixed parameters</p></th>
<th>Required</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>ses_basket[0][quantity]</code></pre></td>
<td><div class="content-wrapper">
<p><span class="status-macro aui-lozenge aui-lozenge-success">YES</p>
</td>
<td><br />
</td>
<td><p>The quantity to be ordered. If empty the eZ Commerce will use 1 instead,</p>
<p>as long as the parameter ses_basket[0][ses_ignore_quantity] is not set.</p></td>
</tr>
<tr>
<td><pre><code>ses_basket[0][ses_ignore_quantity]</code></pre>
<pre><code></code></pre></td>
<td><br />
</td>
<td>1</td>
<td>If set, eZ Commerce will not use 1 instead of empty quantity.</td>
</tr>
<tr>
<td><pre><code>ses_basket[0][sku]</code></pre></td>
<td><div class="content-wrapper">
<p><span class="status-macro aui-lozenge aui-lozenge-success">YES</p>
</td>
<td><br />
</td>
<td>The sku to be ordered. There has to be a valid CatalogElement for the given sku.</td>
</tr>
<tr>
<td><pre><code>ses_basket[0][isVariant]</code></pre></td>
<td><div class="content-wrapper">
 <span class="status-macro aui-lozenge aui-lozenge-current">YES FOR VARIANTS
</td>
<td><pre><code>isVariant</code></pre></td>
<td>Should be set if product is a variant.</td>
</tr>
<tr>
<td><pre><code>ses_basket[0][ses_variant_code]</code></pre></td>
<td><div class="content-wrapper">
 <span class="status-macro aui-lozenge aui-lozenge-current">YES FOR VARIANTS
</td>
<td><br />
</td>
<td><p>Variant code of the ordered variant.</p>
<p><br />
</p></td>
</tr>
<tr>
<td><pre><code>ses_basket[0][updateNotPermitted]</code></pre></td>
<td><br />
</td>
<td>1</td>
<td><p>Can be optionally passed, if you want to avoid, that the line with given sku will be updated.</p>
<p>If set, a new basket line will be created every time when user adds the product with the same sku to the basket.</p></td>
</tr>
<tr>
<td>Additional parameters</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>Example: ses_basket[0][remark]</code></pre></td>
<td><br />
</td>
<td><br />
</td>
<td><div class="content-wrapper">
<p>Any other parameter, that is set in the proper way will be passed to the basket and stored in the basketLine.remoteDataMap.</p>
<p>The values are forwared to the priceRequest/ERP as well!</p>
<p><br />
</p>
<p><strong>Template</strong>:</p>
<p>Please note that the form tag which is used for the basket box only has to be moved to the product.twig.html template. In addition a div "&lt;div class="js_add_to_basket_parent"&gt;" has to be added in order to tell javascript to collect this data as well.</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code> &lt;form method=&quot;post&quot; action=&quot;{{ path(&#39;silversolutions_add_to_basket&#39;) }}&quot;&gt;
     &lt;div class=&quot;js_add_to_basket_parent&quot;&gt;
      &lt;input type=&quot;hidden&quot; name=&quot;ses_basket[0][set_18508050]&quot; value=&quot;18508050&quot;&gt;
 
     &lt;/div&gt;
  &lt;/form&gt;</code></pre>
<p><strong>Data sent to the ERP:</strong></p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>&lt;LineItem&gt;
  ....
  &lt;SesExtension&gt;
       
      &lt;set_18508050&gt;18508050&lt;/set_18508050&gt;
       
  &lt;/SesExtension&gt;</code></pre>

<ul>
<li>It is not possible to use arrays in this case (e.g.ses_basket[0][list][0])</li>
<li>The key (here remark) must be a string and follow the rules for tag names since the key is converted to a tag later on when a price request will be done</li>
</ul>
</td>
</tr>
</tbody>
</table>

### How to add one product to the basket?

**Simple example using a standard POST form**

``` 
<form method="post" action="{{ path('silversolutions_add_to_basket') }}">
<div class="js-add-to-basket-parent"> 
    <div class="space_top space_bottom">{{ 'Quantity:'|trans }}<span class="float_right">
        <input size="10" type="text" name="ses_basket[0][quantity]" class="tooltip" data-powertip="Minimum 1 piece" data-placement="e">

    <input type="hidden" name="ses_basket[0][sku]" value="{{ catalogElement.sku }}" >
    <input type="submit" class="button js-add-to-basket float_right" value="{{ 'Add to basket'|trans }}">
 
</form>
```

### How to add more products to the basket?

It is possible to add more than one product to the basket (e.g. in a product list) by using the index

    ses_basket[0]

    ses_basket[1]

    ...

If you want to add more products to the basket (e.g. from wishlist), you  need one form around all lines, that will add all lines to the basket at once, but you might also need to add only one single product from the list.

To add single product to the basket, you need to define parent elements with the class .js-product-line

### Example for using Ajax 

You need to define one parent element with the class .js-add-to-basket-parent

Important\!

    One of these elements have to be placed inside a form!
    <form>
    .js-add-to-basket - use if you want to add one product to the basket.

    .js-add-all-to-basket - use if you want to add more products to the basket at once

    </form>

**Example using Ajax**

``` 
{% extends "SilversolutionsEshopBundle::pagelayout.html.twig"|st_resolve_template %}

{% block all_content %}

   Name: {{ catalogElement.name }} <br/>
   Name: {{ catalogElement.sku }}

   <div class="js-add-to-basket-parent">
       <input type="hidden" name="ses_basket[0][quantity]" value="1">
       <input type="hidden" name="ses_basket[0][sku]" value="{{ catalogElement.sku }}">
       <button class="button tiny js-add-to-basket" type="submit" data-sku="{{ catalogElement.sku }}">
        <i class="fa fa-cart-plus fa-lg fa-fw"></i>
      </button>

{% endblock %}
```

### Adding additional attributes to the basketline

Please note that an additional parameters should be a scalar value. Arrays are not supported. 

``` 
   Attendee 1 (email)<input type="text" name="ses_basket[0][email_1]" value="" /><br/>
   Attendee 2 (email)<input type="text" name="ses_basket[0][email_2]" value="" /><br/>
```

Display the values in the basket template:

``` 
   Attendee 1 {{ line.remoteDataMap.email_1 }}<br>
   Attendee 2 {{ line.remoteDataMap.email_2 }}
```
