# Catalog - Configuration 

## Configure templates to be used

<table>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>silver_eshop.default.catalog_template.CatalogNode: Catalog:catalog.html.twig
silver_eshop.default.catalog_template.OrderableProductNode: Catalog:product.html.twig
silver_eshop.default.catalog_template.VariantProductNode: Catalog:product_variants.html.twig
silver_eshop.default.catalog_template.ProductType: Catalog:productType.html.twig

</code></pre>
</td>
<td><ul>
<li>Defines which template should be used in case the CatalogController has to render a
<ul>
<li>CatalogNode (category)</li>
<li>OrderableProductNode (a product)</li>
<li>VariantProductNode (a variant)</li>
<li>ProductType (special kind of "variant")</li>
</ul></li>
</ul></td>
</tr>
</tbody>
</table>

## Configuration for variants

See document [VariantType](VariantType_23560699.html)

## Configure the dataprovider 

The following example shows the configuration used for the dataprovider which uses the contentmodel of eZ Platform to store products. 

<table>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<p><strong>see config_data_provider_ez.yml</strong></p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>services:
    siso_search.search_service.product:
        alias: &#39;siso_search.ezsolr_search_service&#39;
    siso_search.search_service.catalog:
        alias: &#39;siso_search.ezsolr_search_service&#39;
parameters:
    silver_eshop.default.catalog_data_provider: ez5</code></pre>
</td>
<td><ul>
<li>Defines which searchservice has to be used</li>
<li>Defines the identifier for the dataprovider (here ez5)</li>
</ul></td>
</tr>
<tr>
<td><div class="content-wrapper">
<p>see /ez_navigation.yml</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.navigation.catalog:
    # the class id has to be specified here
    types: [&#39;ses_category&#39;]
    sections: [1, 2]
    enable_priority_zero: true
    label_fields: [&#39;ses_category_ses_name_value_s&#39;,&#39;name_s&#39;]
    additional_fields: [&#39;ses_category_ses_code_value_s&#39;, &#39;ses_category_ses_name_value_s&#39; ]

# enable if the depth of the node itself has to be added to the depth of the product catalog
siso_core.default.navigation.enable_add_product_catalog_node_depth: true
</code></pre>
</td>
<td><p>The navigation settings are used to build the navigation tree.</p>
<p>For more details see: <a href="Navigation_23560821.html">Navigation</a></p>
<p><br />
</p></td>
</tr>
</tbody>
</table>

In addition the parameters for search have to be defined depending on the dataprovider (see econtent\_search.yml or [econtent - Configuration](econtent---Configuration_23561029.html)).

## Configuration for the CatalogFactory

The following example shows the configuration used for the CatalogFactory to determine which method will create the CatalogElements depending of the ContentType

<table>
<tbody>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>silver_eshop.default.catalog_factory.ses_category: createCatalogNode
silver_eshop.default.catalog_factory.ses_productcatalog: createProductCatalogNode
silver_eshop.default.catalog_factory.ses_product: createOrderableProductNode
silver_eshop.default.catalog_factory.ses_product_type: createProductTypeNode</code></pre>
</td>
<td><ul>
<li>ses_category .. ses_product_type are the identifiers used in the corresponding ContentTypes</li>
<li>createCatalogNode defines e.g. that in case a ContentType "ses_category" is provided the method "createCatalogNode" will be used to create the CatalogElement</li>
</ul></td>
</tr>
</tbody>
</table>

Configuration for variants

Please see [VariantType](VariantType_23560699.html)

## Filter for creating product lists and more

The following configuration  is use to filter eZ ContentObjects. The dataprovider will use these filtes to determine which ContentTypes in eZ Platform are used e.g. for 

  - navigation (usually the ses\_category)
  - product list (display all products belonging to a category)

``` 
silver_eshop.default.ez5_catalog_data_provider.filter:
    navigation:
        contentTypes: [ "ses_category" ]
        sortClauses:
            -
                clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
            -
                clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\DatePublished"
                order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_ASC"
        limit: 20

    category_names:
        contentTypes: [ "ses_category", "ses_product_type" ]
        sortClauses:
            -
                clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
            -
                clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\DatePublished"
                order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_ASC"
        limit: 20
    navigation_incl_products:
        contentTypes: [ "ses_category", "ses_product" ]
        sortClauses:
            -
                clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
            -
                clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\DatePublished"
                order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_ASC"
        limit: 20
    catalogList:
        contentTypes: ["ses_category"]
        sortClauses:
            -
                clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
    productList:
        contentTypes: ["ses_product"]
        sortClauses:
            -
                clause: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query\\SortClause\\Location\\Priority"
                order: "\\eZ\\Publish\\API\\Repository\\Values\\Content\\Query::SORT_DESC"
```

## Misc

<table>
<thead>
<tr class="header">
<th>Setting</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>silver_eshop.default.catalog_product_list_limit: 6
silver_eshop.default.catalog_category_limit: 100
silver_eshop.default.catalog_product_list_limit_ajax: 3</code></pre>
</td>
<td><p><br />
</p>
<ul>
<li>number of elements do be displayed and paging is used</li>
<li>number of elements on category overview pages</li>
<li>number of elements for ajax calls</li>
</ul></td>
</tr>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>silver_eshop.default.last_viewed_products_in_session_limit: 10
silver_eshop.default.catalog_description_limit: 50</code></pre>
<pre><code></code></pre>
<pre><code></code></pre>
</td>
<td><ul>
<li>Max. number of products stored in the last viewed cache</li>
<li>Number of chars used for product descriptions on overview page</li>
</ul></td>
</tr>
</tbody>
</table>
