#  How to search for products 

This examples is using the product search API in order to search products using a searchterm:

``` 
 /**
     * Returns all products
     * @param $offset
     * @param $limit
     * @patram $locationId
     * @return \Siso\Bundle\SearchBundle\Api\Catalog\ProductSearchResult
     */
    private function getAllProducts($offset, $limit, $queryString, $locationId = 2)
    {
        $searchService = $this->get('siso_search.search_service.product');
        $searchGroup =  $this->container->get('ezpublish.config.resolver')->getParameter('groups.product_list', 'siso_search');
        $searchContextService = $this->get('siso_search.search_context_service');
        $searchContext = $searchContextService->getContext();
        $facetService = $this->get('siso_search.facet_service.simple_field');

        $query = new EshopQuery();
        if ($queryString != '' ) {
            $query->addCondition(
                new SearchTermCondition(
                    array(
                        SearchController::SEARCH_CONDITION_TERM => $queryString
                    )
                )
            );
        }
        $query->addCondition(
            new ContentTypesCondition(
                array(
                    SearchController::SEARCH_CONDITION_CONTENT_TYPES =>
                        $searchGroup[SearchController::PRODUCT_LIST_GROUP][SearchController::SEARCH_CONDITION_CONTENT_TYPES]
                )
            )
        );
        if ($locationId > 2) {
            /** @var CatalogDataProviderService $catalogService */
            $catalogService = $this->get('silver_catalog.data_provider_service');
            $catalogProvider = $catalogService->getDataProvider();
            $catalogElement = $catalogProvider->fetchElementByIdentifier($locationId);
            $query->addCondition(
                new SubtreeCondition(
                    array(
                        'path' => implode('/',$catalogElement->path )
                    )
                )
            );
        }
        $formData = array();
        // Add facets
        $productFacets = $facetService->buildFacets($formData, 'product_list');
        foreach($productFacets as $productFacet) {
            $query->addFacet($productFacet);
        }

        $query->setOffset($offset);
        $query->setLimit($limit);
        return $searchService->searchProducts($query, $searchContext);
    }
```
