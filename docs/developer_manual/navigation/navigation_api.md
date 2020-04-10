# Navigation API

## Configuration

``` yaml
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

#### How to enable or disable the whole product catalog?

If for some reason the whole catalog should not be visible in the navigation there is a simple way to do that without going into the eZ Backend. We just need to set the parameter below to false.

``` yaml
siso_core.default.product_catalog_enabled: false
```

#### How to change the caching time period for the navigation?

If you want to change the time that navigation is cached, you can adjust a parameter below to any value you want (in seconds).

``` yaml
siso_core.default.router_cache_ttl: 86400
```

#### How to modify the main navigation location?

The parameter below is an entry root location point for the whole navigation in eZ Backend. Most commonly this value should be set to 2 as it is a location of the content structure.

``` yaml
siso_core.default.navigation_ez_location_root: 2
```

#### How to modify the main navigation depth?

This parameter is responsible for the main navigation depth. Only until this depth the content from eZ Backend will be fetched. This does not include the product catalog, which has it's own depth specified.

``` yaml
siso_core.default.navigation_ez_depth: 3
```

#### How to change the sorting of the navigation?

By default only sorting by priority is available. The user can choose to sort ascending or descending

``` yaml
siso_core.default.navigation_sort_order: 'asc'
```

### Fetching of Content from eZ and Product Catalog from Data Providers

#### How to change content types fetched in main navigation in eZ ?

It may happen in the project that some additional content type needs to be fetched and put into navigation like "blog". In this case we need to extend the parameter below:

``` 
types: ["st_module", "folder", "article", "landing_page", "ses_productcatalog", "blog"]
```

#### How to fetch additional sections?

It can happen in the project that different sections id's are present. In order to fetch them modify parameter

``` 
sections: [1, 2]
```

#### How to modify fetching by priority?

In some projects there is a need to fetch all content types, even if they can priority set to 0. If the parameter below is set to false, then this content will not be fetched:

``` 
enable_priority_zero: false
```

#### How to modify the name of the navigation node?

If you want to modify the name, so different field is displayed as a navigation node label, you should change the parameter below. It has to exist in solr indexed data.

``` 
label_fields: ['name_s']
```

#### How to get additional information about the navigation node?

If in project some additional information is necessary to fullfill the requirements then it can be added as additional field. It has to exist in solr indexed data.

``` 
additional_fields: ['ses_category_ses_code_value_s', 'ses_category_ses_name_value_s']
```

## NavigationHelper

Example how to use the navigation helper in order to build a navigation.

``` php
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
