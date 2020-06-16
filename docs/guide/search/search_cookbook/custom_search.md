# Implement custom search

This example shows how to create a new service which searches for products that:

- are discontinued
- are visible

It sets the sorting option to `relevance`, adds a Manufacturer facet,
and boosts some of the fields (they will be more relevant than others).

The built-in implementation of search interface has all methods required for searching using the Solr gateway.

``` php
class EzSolrEshopSearch implements EshopSearchInterface
```

## Create search method

Create a new method in the controller called `searchDiscontinuedProducts()`.

Use the existing `searchProducts()` method as a basis. Adjust it to use a new parameter for filtering only discontinued products:

``` php
class myController extends BaseController
{
    public searchDiscontinuedProducts() {
         
    }
}
```

## Implement the controller action

1. Inject the search service into the controller.
1. Create a new `EshopQuery` object. This object has all the parameters of the new Solr query.
`EshopQuery` supports different filters/conditions, which are described below.
To get discontinued product, use `SearchTermCondition`, which has the search phrase and the specific field.
In this case, in the current data model, all discontinued fields have the word `DECLINE` in Solr field `ses_product_ses_discontinued_value_s`.
1. Add a condition for visibility, to make sure only visible products are returned.
1. Add boosting so that SKU and name are more important than long description.
    1. The field boosting condition is implementation dependent. This means that the field name values which are passed to the constructor are different for eContent and content model search.
    1. For content model data, you need to pass the content Field identifier strings.
    1. For eContent you need to pass the raw Solr field names, including the default search field (which is `text` by default).
1. Additionally add offset and limit for pagination purposes.
1. Set sorting by relevance.
1. Add product facet option for manufacturers.
1. Now perform the search with the new parameters.
1. The number of total hits is available in `numFound` property.
1. The search results is available in the `resultLines` array.
1. Facets for search result are available in the `facets` array.

``` php
class myController extends BaseController
{
    public searchDiscontinuedProductsAction() {
         
        // 1. First we inject the search and facet service
        $searchService = $this->get('siso_search.ezsolr_search_service');
  
        // 2. Now we create the query with all the parameters we want.
        $eshopQuery = new EshopQuery();
        $eshopQuery->addCondition(new SearchTermCondition(array(
            'searchTerm' => 'DECLINE',
            'fieldRestrictions' => 'ses_product_ses_discontinued_value_s'
        )));
  
        // 3. Add visibility
        $eshopQuery->addCondition(new VisibilityCondition(array('visibility' => 0)));
  
        // 4.b Add boosting (eZ Content)
        $boosts = array(
            'ses_name' => 50,
            'ses_sku' => 100
            'ses_intro' => 20
            'ses_short_description' => 2
            'ses_long_description' => 1.5
            'ses_manufacturer' => 2
        );
        // 4.c Add boosting (Econtent)
        $boosts = array(
            'text' => 1,
            'ses_product_ses_name_value_t' => 50,
            'ses_product_ses_sku_value_s' => 100
            'ses_product_ses_intro_value_t' => 20
            'ses_product_ses_short_description_value_t' => 2
            'ses_product_long_description_value_t' => 1.5
            'ses_product_ses_manufacturer_value_s' => 2
        );
        $eshopQuery->addCondition(new FieldBoosting(array('boost' => $boosts)));
  
        // 5. Add offset and limit
        $eshopQuery->setOffset(0);
        $eshopQuery->setLimit(10);
  
        // 6. Add sorting option by relevance
        $eshopQuery->setSortCriteria(array(new RelevanceSorting()));
  
        // 7. Add product facet option
        $productFacet = new SimpleProductFieldFacet(
            array(
                'fieldName' => 'manufacturer',
                'labelKey' => 'manufacturer',
                'filterType' => 'multi_or',
                'sortType' => 'alpha',
            )
        );
        $eshopQuery->addFacet($productFacet);
        
        // 8. Perform the search
        $productSearchResult = $searchService->searchProducts($eshopQuery, new SearchContext());
  
        // 9. The hits can be found here
        $numberOfHits = $productSearchResult->numFound;
  
        // 10. The search results can be found in this array:
        $productSearchResult->resultLines
  
        // 11. The search result facets can be found in this array:
        $productSearchResult->facets
    }
}
```

### Conditions API

`ContentTypesCondition` - filters results by Content Type. Valid Content Types are for example `ses_product`, `ses_category` or `ses_content`.

`SearchTermCondition` - filters results by phrase, potentially in a specific field. For example: search for `SE10000` in field `sku`:

``` php
$myQuery->addCondition(new SearchTermCondition(array(
    'searchTerm' => 'SE10000',
    'fieldRestrictions' => 'ses_product_ses_sku_value_s'
)));
```

`SectionCondition` - filters results by Section ID. Returns only elements that belong to the given Section.

`SubtreeCondition` - filters results by setting a path. Only elements under this path are fetched.

`VisibilityCondition` - filters results that are shown or hidden.

### Boosting API

`FieldBoosting` - defines which fields should be boosted in search result. They have higher priority in search.

### Sorting API

`RelevanceSorting` - sorts the results by relevance, which is internal Solr implementation for the `score` field.

`ProductFieldSorting` - sorting for products. Supports sorting by name, SKU or price in a given direction.

`ContentNameSorting` - sorts the results by content Field name in a given direction.

### Facets API

`SimpleProductFieldFacet` - facet for specific product Field.

`ProductSpecificationsFieldFacet` - custom facet for product specification information, stored in Matrix Field Type.
