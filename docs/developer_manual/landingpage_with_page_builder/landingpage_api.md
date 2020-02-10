# Landingpage - API 

# Last viewed products

## Technical details

The products are recorded using a javascript call. In order to use it on other pages as well you have to insert this code in your template:

|                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------------------------- |
| `{{ render_hinclude( controller( ``"SilversolutionsEshopBundle:Catalog:storeProductAsLastViewed"``, {``"sku"``: catalogElement.sku} ) ) }}` |

After each call the cache for the  lasted viewed product slider will be purged. By default the view is rendered by an ESI block and cached per user. When the customer visits a new product the list will be purged and regenerated next time it is displayed. 

The products are stored in the session. 

### How to display the products:

<table>
<tbody>
<tr>
<td><p><code class="sourceCode php">{{ render_esi<span class="ot">(</code><br />
<code class="sourceCode php">   </code><code class="sourceCode php">controller<span class="ot">(</code><br />
<code class="sourceCode php">      </code><code class="sourceCode php"><span class="st">&#39;SilversolutionsEshopBundle:EzFlow:showLastViewedProducts&#39;</code><code class="sourceCode php"><span class="ot">,</code><br />
<code class="sourceCode php">       </code><code class="sourceCode php">{ }</code><br />
<code class="sourceCode php">   </code><code class="sourceCode php"><span class="ot">))</code><br />
<code class="sourceCode php">}}</code></p></td>
</tr>
</tbody>
</table>

The controller is able to render a different template if required (parameter template). By default "SilversolutionsEshopBundle:Catalog:slider.html.twig" will be used. 

The caching strategy can be defined in the config file. Since it is dynamic vary: cookie should be used\!

<table>
<tbody>
<tr>
<td><p><code class="sourceCode php">silver_eshop.</code><code class="sourceCode php"><span class="kw">default</code><code class="sourceCode php">.http_cache:</code><br />
<code class="sourceCode php">       </code> <br />
<code class="sourceCode php">        </code><code class="sourceCode php">last_viewed_products:</code><br />
<code class="sourceCode php">            </code><code class="sourceCode php">max_age: <span class="dv">36000</code><br />
<code class="sourceCode php">            </code><code class="sourceCode php">vary: cookie</code></p></td>
</tr>
</tbody>
</table>

The amount of stored products can be defined by configuration as well and is set to 10 by default:

|                                                                      |
| -------------------------------------------------------------------- |
| `silver_eshop.``default``.last_viewed_products_in_session_limit: 10` |

# Bestsellers

The Bestseller block is rendered using content id (if a product category has been selected) or a category id (if the input field has been used).

If the selected object is not a category an error message will be displayed. 

Please check if the Bestseller feature has been enabled:

``` 
siso_core.default.enable_bestsellers: true
```
