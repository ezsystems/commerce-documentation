#  Templating 

![](http://ezcommerce-demo.ezplatform.com/bundles/silversolutionseshop/img/logo.png)

## Introduction

eZ Commerce uses Twig templates for the frontend and backend features as well. Twig is a very powerful template language which allow a very flexible way to change the frontend of the shop and platform. 

All Template can be extended or overridden in projects if required. 

For a general documentation about the template language we recommend the documentation from Symfony:

 <http://twig.sensiolabs.org/>

## Provided template functions

### Functions

<table>
<thead>
<tr class="header">
<th>Operator</th>
<th>Features</th>
<th>Docu</th>
</tr>
</thead>
<tbody>
<tr>
<td>st_tag</td>
<td>Injects data tags for google tag manager or BDD tests</td>
<td><a href="st_tag---selector-tagging-for-Behat-and-Google-Tag-manager_23560394.html">st_tag - selector tagging for Behat and Google Tag manager</a></td>
</tr>
<tr>
<td>ses_render_field</td>
<td>renders a product field</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td>ses_navigation</td>
<td>Returns navigation node tree</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td>ses_product</td>
<td>Returns a product node</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td>ses_variant_product_by_sku</td>
<td>Fetches a VariantProductNode for given sku</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td><pre><code>ses_assets_main_image</code></pre></td>
<td>Returns the main image of catalogElement or null</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td><pre><code>ses_assets_image_list</code></pre></td>
<td>Returns the list of images for a catalogElement or an empty array</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td><pre><code>ses_variants_sort</code></pre></td>
<td>Returns all product variants in a given order</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td><pre><code>ses_render_price</code></pre></td>
<td>Renders a <a href="http://www.extranet.silversolutions.de:8090/display/EX/PriceField" class="external-link">PriceField</a></td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td><pre><code>ses_render_stock</code></pre></td>
<td>Renders a <a href="http://www.extranet.silversolutions.de:8090/display/EX/StockField" class="external-link">StockField</a></td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td><pre><code>ses_render_specification_matrix</code></pre></td>
<td>Renders a specification MatrixField</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
</tbody>
</table>

### Filters

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Name
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>date_format</code></pre></td>
<td>Formats a DATE value</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td><pre><code>price_format</code></pre></td>
<td><p>Formats a price value</p>

<p>default parameters in silver.eshop.yml</p>
<span class="aui-icon aui-icon-small aui-iconfont-info confluence-information-macro-icon" style="color: rgb(74,103,133);"> 

<p>silver_eshop.default.price_format_currency: EUR</p>
<p>silver_eshop.default.price_format_locale: en</p>

</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td><pre><code>youtube_video_id</code></pre></td>
<td>Returns a video ID from a Youtube URL</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td>shipping</td>
<td>Returns the shipping costs from the basket or null</td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
<tr>
<td>code_label</td>
<td><p>Returns the translated code label. This can be e.g. country code or different code like mrs.<br />
if the context is 'country' first Symfony Intl Bundle is used to resolve the country code to country name otherwise just the translation for the code is returned</p></td>
<td><a href="Twig-extension_23560696.html">Twig extension</a></td>
</tr>
</tbody>
</table>

## Template resolver - a flexible template solution

eZ Commerce comes with a very flexible template resolver system which allows to override templates on a siteaccess and design base. 

Please check the documentation:

[Template resolver](Template-resolver_23560645.html)

## eZ Design Engine

eZ Commerce is available and can be used instead of the Template resolver.

Please check the following documentations for detailed information:

  - [Integration of eZ Design Engine](eZ-Design-Engine_23560210.html)
  - [eZ Design Engine documentation](https://doc.ezplatform.com/en/latest/guide/design_engine/)

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2017-03-14 um 14.29.19.png](attachments/23560960/23563991.png) (image/png)  
