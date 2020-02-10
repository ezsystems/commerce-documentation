#  Bestseller - UI 

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
<td>EshopBundle/Resources/views/Bestsellers/bestsellers.html.twig</td>
<td><div class="content-wrapper">
<p>Renders a bestseller page</p>
<p><img src="attachments/23560829/23563929.png" class="confluence-embedded-image" height="250" /></p>
</td>
</tr>
<tr>
<td>EshopBundle/Resources/views/Bestsellers/bestsellers_box.html.twig</td>
<td><div class="content-wrapper">
<p>Renders a slider for a st_module (100% or 50%)</p>
<p><img src="attachments/23560829/23563931.png" class="confluence-embedded-image confluence-thumbnail" height="250" /></p>
</td>
</tr>
<tr>
<td>EshopBundle/Resources/views/Bestsellers/bestsellers_catalog.html.twig</td>
<td><div class="content-wrapper">
<p>Renders a slider for the catalog page</p>
<p><img src="attachments/23560829/23563930.png" class="confluence-embedded-image confluence-thumbnail" height="150" /></p>
</td>
</tr>
<tr>
<td>EshopBundle/Resources/views/Bestsellers/bestsellers_box_esi.html.twig</td>
<td><div class="content-wrapper">
<p>Creates "Edge Side Includes" Tag and calls Controller for landing page bestsellers</p>
</td>
</tr>
</tbody>
</table>

Category pages/Caching

Catalog bestsellers are cached independent of the rest of the page using ESI.

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/catalog.html.twig**

``` 
{{ render_esi( controller( 'SilversolutionsEshopBundle:Bestsellers:getCategoryBestsellers',
{
    'locationPath': catalogElement.path,
    'locationName': catalogElement.name,
} )) }}
```

## Content and Translations

  - Textmodule: bestsellers\_intro\_text

## Related routes

``` 
silversolutions_bestsellers:
    path:      /bestsellers
    defaults:
        _controller: SilversolutionsEshopBundle:Bestsellers:getBestsellersList
        breadcrumb_path: silversolutions_bestsellers
        breadcrumb_names: Bestsellers
```

## Attachments:

![](images/icons/bullet_blue.gif) [image2017-3-20 8:51:10.png](attachments/23560829/23563969.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2017-3-20 8:53:6.png](attachments/23560829/23563967.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2017-3-20 8:55:28.png](attachments/23560829/23563971.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2017-3-22 16:1:35.png](attachments/23560829/23563929.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2017-3-22 16:2:38.png](attachments/23560829/23563931.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2017-3-22 16:5:46.png](attachments/23560829/23563930.png) (image/png)  
