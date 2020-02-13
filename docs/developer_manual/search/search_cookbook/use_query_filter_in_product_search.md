# Use query filter in product search 

## Introduction

Filter by a search engine field in PHP.

## Example Filter by type and brand

```
use Siso\Bundle\SearchBundle\Api\SearchContext;
use Siso\Bundle\SearchBundle\Api\EshopQuery;
use Siso\Bundle\SearchBundle\Controller\SearchController;
$query = new EshopQuery();

$queryString = 'content_type_id_id:2 AND ses_product_ses_datamap_ses_brand_value_s:HP';

$query->addCondition(
    new SearchQueryCondition(
        array(SearchController::SEARCH_CONDITION_QUERY => $queryString)
    )
);
$searchService = $this->getContainer()->get('siso_search.search_service.product');
$result = $searchService->searchProducts($query, new SearchContext());
```
