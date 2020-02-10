#  How to: Change catalog layout 

Please make sure before you start to install the [boilerplate](Extending-the-frontend_23561043.html) first

## Introduction

In this guide we want to change the catalog layout in an easy way.

<table>
<thead>
<tr class="header">
<th>Before</th>
<th>After</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<p><img src="attachments/23560224/23570985.png" class="confluence-embedded-image confluence-content-image-border" width="550" /></p>
</td>
<td><div class="content-wrapper">
<p><img src="attachments/23560224/23570986.png" class="confluence-embedded-image confluence-content-image-border" width="550" height="418" /></p>
</td>
</tr>
</tbody>
</table>

We want to hide the Side Navigation and make the Product Catalog full width.

Lets start with finding the right Template in the vendor section:

Please copy the template "**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/catalog.html.twig**" to "**app/Resources/Views/Catalog/catalog.html.twig**"

You have to create the folder Catalog before you can copy the template

Delete the content thats inside the block **second\_nav**

**Before**

``` 
{% block second_nav %}
            <div class="large-3 columns show-for-large-up u-padding-right-3x hide-for-print">
                {% set product_catalog_id = get_parent_product_catalog(catalogElement) %}
                {% if product_catalog_id|default is not empty %}
                    {# @notice - this will display the whole catalog and the current catalog element will be marked as active #}
                    {% include 'SilversolutionsEshopBundle:Navigation:left_navigation.html.twig'
                    with { 'locationId': product_catalog_id} %}
                {% endif %}
            
{% endblock %}
```

Change it to:

**After**

``` 
{% block second_nav %}
           
{% endblock %}
```

  - change large-9 to large-12 ... TODO

If you dont see any changes please remove the cache\!

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-07-26 um 12.32.25.png](attachments/23560224/23570985.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2018-07-26 um 12.33.32.png](attachments/23560224/23570986.png) (image/png)  
