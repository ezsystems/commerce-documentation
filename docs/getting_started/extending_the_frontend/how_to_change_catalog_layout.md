# How to: Change catalog layout

Please make sure before you start to install the [boilerplate](extending_the_frontend.md) first

## Introduction

In this guide we want to change the catalog layout in an easy way.

Before:

![](../img/extending_the_frontend_2.png)

After:

![](../img/extending_the_frontend_3.png)

We want to hide the Side Navigation and make the Product Catalog full width.

Lets start with finding the right Template in the vendor section:

Please copy the template `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/catalog.html.twig` to `app/Resources/Views/Catalog/catalog.html.twig`

You have to create the folder Catalog before you can copy the template

Delete the content thats inside the block `second_nav`

``` html+twig
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

``` html+twig
{% block second_nav %}
           
{% endblock %}
```

!!! note

    If you don't see any changes please remove the cache\!
