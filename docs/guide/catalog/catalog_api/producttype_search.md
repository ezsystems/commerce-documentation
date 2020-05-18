# ProductType search

The `ProductType` catalog element can appear in the search result of products.

For this to happen you need to modify the configuration by adding `ses_product_type` to the product list or to product search.

`use_display_in_product_list_flag` enables or disables the use of the `productType` flag when displaying in search and in product list:

`use_display_in_product_list_flag: true`

Additionally, a Product Type indexes all data from its products.
You can disable this using the following configuration:

`cp_to_product_type: false`

Disabling this lowers indexer times but product data is not indexed alongside Product Types.

## Template

Search results display Product Types using the `src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/listProductTypeNode.html.twig` template.

The `get_search_result_template` Twig function selects the correct template:

``` php
{# Search result of products, product types and categories #}
{% set catalogElement = content %}
{{ get_search_result_template(catalogElement) }}
```

``` php
/**
 * Selects the correct template according to catalog element type
 *
 * @param $catalogElement
 * @return string rendered template
 */
public function getSearchResultTemplate($catalogElement)
{

    switch ($catalogElement) {
        case ($catalogElement instanceof ProductNode):
            $templateName = 'SilversolutionsEshopBundle:Catalog:listProductNode.html.twig';
            break;
        case ($catalogElement instanceof ProductType):
            $templateName = 'SilversolutionsEshopBundle:Catalog:listProductTypeNode.html.twig';
            break;
        default:
            $templateName = 'SilversolutionsEshopBundle:Catalog:listCatalogNode.html.twig';
            break;
    }
    $html = $this->environment->render(
        $this->getResolvedView($templateName),
        array('catalogElement' => $catalogElement)
    );

    return $html;
}
```
