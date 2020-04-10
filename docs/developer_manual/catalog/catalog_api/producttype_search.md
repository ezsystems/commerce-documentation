# ProductType search

## Introduction and configuration

The Product Type catalog element can appear in the search result of products. Now products and product types can appear in the same search result.

For this to happen you can modify the configuration adding ses\_product\_type to the product list or to the product search

There are two additional configuration that will enable or disable the use of the prouctType flag display in search and display in product list

use\_display\_in\_search\_flag: true

`use_display_in_product_list_flag: true`

Additionally a product type will index all the data from its products. However this can be disabled using the following configuration:

`cp_to_product_type: false` (Disabling this will lower indexer times but product data will not be indexed along product type)

## Template

ProductType has a new template for the search result:

src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/listProductTypeNode.html.twig

In search result a new twig function was implemented to select the correct template.

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
