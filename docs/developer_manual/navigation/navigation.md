#  Navigation 

## Introduction

**Why does eZ Commerce offers a NavigationService?**

eZ Commerce supports mixing of catalog elements (products) provided by a storage engine and content from CMS eZ Platform. As long as product would be stored in eZ Platform the menu could be build by eZ twig functions and controllers. 

Since eZ Commerce offers a flexible way to store products (DataProviders) a universal NavigationService ensures that the dataproviders are used to load the categories for the menu. 

**How to "inject" a product catalogue?**

A product catalogue has to be setup in the backend using a object (product catalog) which introduces the products for the shop (here named as "Product Catalog"). The reason is that product may come from different sources (aka dataproviders). 

**Example**: 

![](attachments/23560821/23563543.png)

## Features

eZ Commerce navigation supports different scenarios:

  - navigation with one ore more catalog trees inside of an eZ Platform content tree.
  - navigation without catalog
  - navigation where the root element is product catalog
  - navigation where the root element is product catalog, but display only part of a catalog (display only sub-elements that belongs to one category)
  - Store any additional data in the navigation node, that you need
  - Sorting the navigation nodes by priority
  - Enable/disable fetching of nodes where the priority is 0
  - Different depth for eZ content and catalog
  - Highlighting of the active node in the navigation
  - HTTP caching  

Limitation

The new navigation does not support product catalogs and categories with several locations. If you want to have e.g. the same categories on several places, you would need to copy the content. Products can be assigned to one or more categories.

## How does it work

### How the data is fetched?

Navigation is using eZ search in order to fetch the eZ content and a custom search service to fetch to catalog data directly from solr. Every dataprovider must ensure, that the data is indexed in the correct format. These data must be indexed at least.

``` 
document_type_id: [content/econtent]
type_id: [class id]
main_location_visible_b: [true/false]
section_id: [section id]
main_location_depth_i: [depth]
main_location_path_id: [path]
main_location_priority_i: [priority]
```

Additionally every data provider is able to extend the search query before the catalog data is fetched from solr. Read more in the [cookbook](Navigation---Cookbook_23560856.html) about it.

### Used fields for navigation

``` 
siso_core.default.navigation.catalog:
        #the class id has to be specified here
        types: ['38']
        sections: [1, 2]
        enable_priority_zero: true
        label_fields: ['ses_category_ses_name_value_s','name_s']
        additional_fields: ['ses_category_ses_code_value_s', 'ses_category_ses_name_value_s' ]
```

The name used for the navigation can be defined by configuration. The parameter "label\_fields" contains a list of attribute names (Solr names) which will be taken as name used in the menu. The first attribute which is available will be used. 

**Important**:

  - The eZ standard attribute name\_s does **not** always contain the **correct translation** (issue of the eZ Indexer). 

  - When name\_s is used in "label\_fields" it might happen that the navigation/menu is not translated

  - Solution:  configure the id of the attribute directly e.g. 
    
        ses_category_ses_name_value_s

### Sorting

The navigation elements are sorted by location priority.

### Caching

The navigation is cached in 2 different ways:

  - The top navigation is cached using http-cache and the strategy defined by "navigation".

  - It is using one cache per siteaccess

  - By default it is cached by user-hash for 10 hours  

  - The left menu is cached in the same way. 

    ``` 
    silver_eshop.default.http_cache:
           navigation:
                max_age: 36000
                vary: user-hash
    ```

When a content is modified which belongs to the navigation a contentmodficationhandler will be triggered. Please make sure that a cronjob is activated which refresh the cache if required:

php bin/console silversolutions:cache:refresh --env=prod

### How the catalog data is injected?

When the product catalog is accessible and the navigation is build, for a special ez content type **`ses_productcatalog`** the catalog elements are injected. The object of the type **`ses_productcatalog`** is the root node for the catalog. You can place as many product catalogs (on any level), as you wish. 

A new product catalog can be assigned to a content tree in eZ Platform using a special Content Type (ses\_productcatalog).

If you are using **eZ** as dataprovider, you have to place your categories and products directly under your ses\_productcatalog and then configure the root\_node id, so it points to itself.

![](attachments/23560821/23563620.png)

If you are using **econtent** as dataprovider, you just have to configure the root\_node it, so it matches the node id of the root element in the sve\_object table.

![](attachments/23560821/23563602.png)

Following fields are defined in the **`ses_productcatalog`** type:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Attribute name
</th>
<th><div class="tablesorter-header-inner">
Attribute identifier
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>name</code></td>
<td>Name of the product catalog for the Navigation</td>
</tr>
<tr>
<td>Root node</td>
<td><code>root_node</code></td>
<td>Root node of the product catalog in the storage engine</td>
</tr>
<tr>
<td>Depth</td>
<td><code>depth</code></td>
<td>The depth of the product catalog. The navigation will be build using this parameter. A depth of 1 means that there will be just one level starting at "Root node"</td>
</tr>
</tbody>
</table>

## Important terms used in this document

Dataprovider

**Dataprovider** is the instance that provides the catalog data. eZ Commerce (Advanced version only) provides a flexible way to handle products and categories. Catalog data can be stored in different source in the shop (eZ, DB, solr) and the main goal of the data provider is to fetch the product data from the source and build the catalog elements.

Currently there are two concrete implementations for dataproviders:

<table>
<tbody>
<tr>
<td><p>Catalog Data CMS eZ</p>
<p>(Ez5CatalogDataProvider)</p></td>
<td><p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" />  products can be edited in the CMS</p>
<p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" />  good for up to 20.000 products</p>
<p><img src="images/icons/emoticons/forbidden.png" class="emoticon emoticon-minus" alt="(minus)" /> import of products costs time</p></td>
<td><p>customer has no PIM system</p>
<p>and the amount of roducts</p></td>
</tr>
<tr>
<td><p>Catalog Data eContent</p>
<p>(EcontentCatalogDataProvider)</p></td>
<td><p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /> allws more than 1 million products</p>
<p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /> fast imports</p>
<p><img src="images/icons/emoticons/forbidden.png" class="emoticon emoticon-minus" alt="(minus)" /> currently no edit interface in the backend of the CMS</p></td>
<td><p>The customer is using a PIM system</p>
<p>or the ERP provides all the product information</p></td>
</tr>
</tbody>
</table>

## Before you start 

Please keep in mind that Navigation is really connected with a lot of different modules in our shop. Be sure to check these out:

  - [Catalog Element](http://confluence.ng.silverproducts.de/)
  - [Dataprovider](Catalog-data-providers---Products-from-different-sources_23560468.html)

## FAQ

I don´t have any product catalog in my application. What do I have to do?

 If you want to build navigation without product catalog, you need to change the configuration.

``` 
parameters:
    #set to false
    siso_core.default.product_catalog_enabled: false 

    siso_core.default.navigation.content:
        #remove "ses_productcatalog" from the list
        types: ["st_module", "folder", "article", "landing_page"] 
        ...
```

I don´t want to display content with priority 0. What do I have to do?

 If you want to fetch only the content, where the priority is higher than 0, you need to change the configuration.

``` 
parameters:   
    siso_core.default.navigation.content:
        #set to false       
        enable_priority_zero: false

    siso_core.default.navigation.catalog:
        #set to false   
        enable_priority_zero: false
```

I want to display a small image instead of the node label. 

 This is possible if you have an image stored in your eZ content. Then you can additionally store the image in the navigation node and output it in the frontend.

``` 
parameters:   

    siso_core.default.navigation.content:
        ...
        additional_fields: ['name', 'short_description', 'show_children', 'image']
```

You need to adapt the template to render the image instead of the label. Example:

**Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/knp\_menu.html.twig**

``` 
{% block label %}

  {% if(item.getExtra('image')) %}
    <img src="{{ item.getExtra('image') }}"/>
  {% else %}
    {{ item.label|raw }}
  {% endif %}

{% endblock %}
```

![](attachments/23560857/23563772.png)

How can I influence sorting of the navigation elements?

The sort order can be configured.

``` 
parameters:
    ...
    siso_core.default.navigation_sort_order: 'asc'
```

For the sorting the priority is used. This is either the priority of the catalog elements (if you are using econtent), or the location priority for eZ.

#### Priority in the sve\_object table

![](attachments/23560857/23563773.png)

#### Location priority in eZ

![](attachments/23560857/23563775.png)

I would like to display 3 levels of categories in the mega menu. What do I have to do?

 Please edit the Product catalog in the backend and change the settings for Depth. In prod mode you might have to clear the cache.

![](attachments/23560857/23561577.png)

## Templating

The Navigation is using a list of templates which can be overridden if required. 

[Read more...](Navigation---Templates_23560868.html)

## Cookbook

Find recipes in [cookbook...](Navigation---Cookbook_23560856.html)

## API

## Configuration

``` 
#full navigation configuration
parameters:    
    siso_core.default.product_catalog_enabled: true
    siso_core.default.product_catalog_identifier: "ses_productcatalog"
    siso_core.default.product_catalog_content_type_id: 44
    siso_core.default.router_cache_ttl: 86400    

    siso_core.default.navigation_ez_location_root: 2
    siso_core.default.navigation_ez_depth: 3
    siso_core.default.navigation_sort_order: 'asc'

    siso_core.default.navigation.content:
        types: ["st_module", "folder", "article", "landing_page", "ses_productcatalog"]
        sections: [1, 2]
        enable_priority_zero: true
        #additional field keys for translating navigation node label
        label_fields: ['name']
        additional_fields: ['name', 'short_description', 'show_children', 'image']

    siso_core.default.navigation.catalog:
        #the class id has to be specified here
        types: ['38']
        sections: [1, 2]
        enable_priority_zero: true
        label_fields: ['name_s']
        additional_fields: ['ses_category_ses_code_value_s', 'ses_category_ses_name_value_s' ]
```

#### How to enable or disable the whole product catalog ?

If for some reason the whole catalog should not be visible in the navigation there is a simple way to do that without going into the eZ Backend. We just need to set the parameter below to false.

``` 
siso_core.default.product_catalog_enabled: false
```

#### How to change the caching time period for the navigation ?

If you want to change the time that navigation is cached, you can adjust a parameter below to any value you want (in seconds).

``` 
siso_core.default.router_cache_ttl: 86400
```

#### How to modify the main navigation location ?

The parameter below is an entry root location point for the whole navigation in eZ Backend. Most commonly this value should be set to 2 as it is a location of the content structure.

``` 
siso_core.default.navigation_ez_location_root: 2
```

#### How to modify the main navigation depth ?

This parameter is responsible for the main navigation depth. Only until this depth the content from eZ Backend will be fetched. This does not include the product catalog, which has it's own depth specified.

``` 
siso_core.default.navigation_ez_depth: 3
```

#### How to change the sorting of the navigation ?

By default only sorting by priority is available. The user can choose to sort ascending or descending

``` 
siso_core.default.navigation_sort_order: 'asc'
```

### Fetching of Content from eZ and Product Catalog from Data Providers

#### How to change content types fetched in main navigation in eZ ?

It may happen in the project that some additional content type needs to be fetched and put into navigation like "blog". In this case we need to extend the parameter below:

``` 
types: ["st_module", "folder", "article", "landing_page", "ses_productcatalog", "blog"]
```

#### How to fetch additional sections ?

It can happen in the project that different sections id's are present. In order to fetch them modify parameter

``` 
sections: [1, 2]
```

#### How to modify fetching by priority ?

In some projects there is a need to fetch all content types, even if they can priority set to 0. If the parameter below is set to false, then this content will not be fetched:

``` 
enable_priority_zero: false
```

#### How to modify the name of the navigation node ?

If you want to modify the name, so different field is displayed as a navigation node label, you should change the parameter below. It has to exist in solr indexed data.

``` 
label_fields: ['name_s']
```

#### How to get additional information about the navigation node ?

If in project some additional information is necessary to fullfill the requirements then it can be added as additional field. It has to exist in solr indexed data.

``` 
additional_fields: ['ses_category_ses_code_value_s', 'ses_category_ses_name_value_s']
```

## NavigationHelper

Example how to use the navigation helper in order to build a navigation.

``` 
public function buildSubnavigationAction(CatalogElement $catalogElement)
{
    $productCatalogLocationId = 65;

    /** @var NavigationHelper $menuBuilder */
    $navigationHelper = $this->get('siso_core.navigation_helper');

    //build navigation for the productCatalog but display only the subcategories of the current catalogElement
    $menu = $navigationHelper->createMenu($productCatalogLocationId, $catalogElement->id);

    //TODO render menu
}

public function buildNavigationAction($locationId)
{
    /** @var NavigationHelper $menuBuilder */
    $navigationHelper = $this->get('siso_core.navigation_helper');

    //build navigation for the given location id
    $menu = $navigationHelper->createMenu($locationId);

    //TODO render menu
}
```
 
## Attachments:

![](images/icons/bullet_blue.gif) [catalog.png](attachments/23560821/23563620.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2016-09-21 um 14.54.59.png](attachments/23560821/23563602.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-10-11 14:33:19.png](attachments/23560821/23563543.png) (image/png)  
