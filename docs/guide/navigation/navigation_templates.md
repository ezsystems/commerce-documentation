# Navigation templates

### How to render the navigation?

#### Left navigation with product catalog as root element

``` html+twig
{# get the parent product catalog of the current catalog element #}
{% set product_catalog_id = get_parent_product_catalog(catalogElement) %}

{% include 'SilversolutionsEshopBundle:Navigation:left_navigation.html.twig'
  with { 'locationId': product_catalog_id}
%}

{# if you want, you can pass the id of the active catalog element, but it is not required, since the active node will be highlited automatically via js #}
{# if you pass th eactive node id, the navigation will be cached for this id #}
{% include 'SilversolutionsEshopBundle:Navigation:left_navigation.html.twig'
  with { 'locationId': product_catalog_id, 'active_node': catalogElement.id}
%}
```

#### Left navigation that renders only part of the catalog

(e.g. subcategories of the current catalog element)

``` html+twig
{# get the parent product catalog of the current catalog element #}
{% set product_catalog_id = get_parent_product_catalog(catalogElement) %}

{# even if you want to display only part of the catalog, the root node still must be the parent product catalog! #}
{% include 'SilversolutionsEshopBundle:Navigation:left_navigation.html.twig'
  with {'locationId': product_catalog_id, 'catalog_subtree': catalogElement.identifier}
%}
```

#### Main navigation

``` html+twig
{% include 'SilversolutionsEshopBundle:Navigation:main_navigation.html.twig'|st_resolve_template
  with {'locationId':ses_config_parameter('navigation_ez_location_root', 'siso_core')}
%}
```

#### Offcanvas navigation

``` html+twig
{% include 'SilversolutionsEshopBundle:Navigation:offcanvas_navigation.html.twig'|st_resolve_template
with {'locationId':ses_config_parameter('navigation_ez_location_root', 'siso_core')}
%}
```

### Templates list

#### Main navigation

|Path|Description|
|--- |--- |
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/main_navigation.html.twig|Renders the navigation subcontroller, that builds the main navigation|
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/main_menu.html.twig|Renders the main navigation list <ul> and includes `knp_menu.html.twig`|
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/knp_menu.html.twig|Renders the main navigation nodes as <li> elements|

#### Left navigation

|Path|Description|
|--- |--- |
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/left_navigation.html.twig|Renders the navigation subcontroller, that builds the left navigation|
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/left_menu.html.twig|Renders the left navigation list <ul> and includes `left_menu.html.twig`|
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/knp_left_menu.html.twig|Renders the left navigation nodes as <li> elements|

#### Offcanvas

|Path|Description|
|--- |--- |
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/offcanvas_navigation.html.twig|Renders the navigation subcontroller, that builds the offcanvas navigation|
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/offcanvas_menu.html.twig|Renders the offcanvas navigation list <ul> and includes `knp_offcanvas.html.twig`|
|Silversolutions/Bundle/EshopBundle/Resources/views/Navigation/knp_offcanvas.html.twig|Renders the offcanvas navigation nodes as <li> elements|


### Related custom Twig modifiers/functions/etc/

|Function|Usage|Parameters|Description|
|--- |--- |--- |--- |
|get_parent_product_catalog|{% set product_catalog_id = get_parent_product_catalog(catalogElement)</br>%}|\Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement|Fetches the parent product catalog element for the $identifier|
|get_data_location_ids|{% set data_locations = get_data_location_ids(location) %}</br>{% set data_locations = get_data_location_ids(catalogElement) %}|\eZ\Publish\API\Repository\Values\Content\Location</br>or</br>\Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement|Returns a comma separated string of data_locations of the given element.</br>All parent locations incl. the element location are returned.</br>These data_locations are used to highlite the active node in the navigation.|
