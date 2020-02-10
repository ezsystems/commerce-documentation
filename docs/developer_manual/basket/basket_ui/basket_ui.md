#  Basket - UI 

## Templates list

<table>
<thead>
<tr class="header">
<th>Path</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><p>SilversolutionsEshopBundle:Basket:show.html.twig</p></td>
<td>Main template</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Basket:messages.html.twig</td>
<td>renders error messages if a basket change was not successful</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Fieldtypes:StockField.html.twig</td>
<td>renders a stock field</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:parts:stock_legend.html.twig</td>
<td><br />
</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Basket:basket_preview.html.twig</td>
<td>Renders a basket preview used e.g. for main navigation</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Basket:stored_basket_preview_wish_list.html.twig</td>
<td>is used for the "My Shop" section and displays a menu item including the number of products in my wishlist</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Basket:stored_basket_preview_comparison.html.twig</td>
<td>is used for the "My Shop" section  and displays a menu item for the comparison feature</td>
</tr>
</tbody>
</table>

## Display the content of a basket

The basket can be displayed using a standard controller of eZ Commerce (/basket/show).  The controller provides a variable basket containing the content of the current basket.

You can access all Basket Attributes from a template:

``` 
{# the count of basket lines #}
{{ basket.lines.count }}

{# Access to basket lines #}
{% for line in basket.lines %}
    {{ line.sku }} 
    {{ line.linePriceAmount }} 
{% endfor %}

{# Access to basket Attributes #}
{{ basket.basketName }}

{# totalsSum is a BasketTotals object #}
{{ basket.totalsSum.totalGross }}

{# totals is an array of BasketTotals objects #}
{% for total in basket.totals %}
    {{ total.totalGross }}
{% endfor %}

{# Access to information that are stored in remoteDataMap #}

{% if line.remoteDataMap.onStock %}
    <strong class="availability tooltip green smaller" data-powertip="{{ 'This product is available for purchase'|st_translate }}">
        {{ 'Available'|st_translate }}
    </strong>
{% else %}
    <strong class="availability tooltip red smaller" data-powertip="{{ 'This product is not available for purchase'|st_translate }}">
        {{ 'Not Available'|st_translate }}
    </strong>
{% endif %}  
```

The following templates are prepared for displaying basket related data:

  - The Basket preview will be rendered using the template Basket/basket\_preview.html.twig.
  - The basket (basket/show) is implemented in the template Basket/show.html.twig

## Available Entity Attributes

see [Basket data model](Basket-data-model_23560234.html)

### CatalogElement

If you need to get the CatalogElement, you can fetch it by {{ line.sku }}. The current implementation stores a copy of given product fields inside a basket line. 

This ensures that the basket get a fast access to the product data and that during a checkout/payment process the product data will be available even when the product is removed from the catalog meanwhile. 

See [Get a ProductNode by SKU](Catalog---UI_23560463.html)

If a basket line does not provide product data (e.g. the caching life time of a product has been exceeded) the product can be fetched using a twig function.  

``` 
{% if line.catalogElement|default is not empty %}
  {% set product = line.catalogElement %}
{% else %}
  {% set product = ses_product({'sku': line.sku, 'variantCode': line.variantCode }) %}
{% endif %}
```

### Basket Preview

The basket preview is rendered using the sub-controller in EshopBundle/Resources/views/pagelayout.html

**pagelayout.html.twig**

``` 
{% block basket_preview %}
    {% render(controller('SilversolutionsEshopBundle:Basket:preview')) %}
{% endblock %} 
```

The responsible sub-controller (previewAction()) fetches the basket from the DB and render it in the template: basket\_preview.html

**basket\_preview.html.twig**

``` 
{% if basket.lines.count > 0 %} 
    <ul class="shopping_basket flyout border round" id="small_basket">
        <li class="first">
            <ul class="shopping_basket_list inset_shadow">
            {% for line in basket.lines %}
                <li class="first">
                    {% set product = ses_product({'sku': line.sku }) %}
                    <span class="amount">{{ line.quantity }} &times;
                    <span class="text"><a href="{{ product.seoUrl }}">{{ product.name }}</a>
                    <span class="price">{{ line.price|price_format(line.currency) }}
                </li>
            {% endfor %}
            </ul>
        </li>
        <li class="last">
            <ul class="basket_total">
                <li class="clearfix">
                    <a href="{{ path('silversolutions_basket_show') }}" class="button float_left">{{ 'Go to Basket'|st_translate }}</a>
                    <span class="price big float_right">{{ basket.totalsSum.totalGross|price_format }}
                    <span class="total float_right">{{ 'Total'|st_translate }}: 
                </li>
            </ul>
        </li>
    </ul>
{% endif %}
```
