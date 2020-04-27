# How to implement a custom sorting handler for econtent

## In this example we will create a custom sorting that will use priority field.

This will create a new sort criteria for use with Solr. You will be able to specify the Solr field name for sort. In this example it will be a field that stores product priority, but it can be any Solr field you want.

## Steps

### 1. Create a new class that extends AbstractSortCriterion

``` php
<?php
 
namespace MyProject\Bundle\ProjectBundle\Api\Search;
 
use Siso\Bundle\SearchBundle\Api\AbstractSortCriterion;
 
class PriorityFieldSorting extends AbstractSortCriterion
{
}
```

### 2. Create new handler class.

Please note that variable $indexName is the Solr field that will be used to sort.

You can choose any field you like.

``` php
<?php
 
namespace MyProject\Bundle\ProjectBundle\Service\Search;
 
 
use Siso\Bundle\SearchBundle\Api\AbstractSortCriterion;
use Siso\Bundle\SearchBundle\Api\SearchClauseInterface;
use Siso\Bundle\SearchBundle\Api\SolariumSearchClauseHandlerInterface;
use Solarium\QueryType\Select\Query\Query;
use MyProject\Bundle\ProjectBundle\Api\Search\PriorityFieldSorting;
 
 
class PriorityFieldSortingHandler implements SolariumSearchClauseHandlerInterface
{
    /**
     * @param SearchClauseInterface $searchClause
     * @param Query $query
     */
    public function handleSearchClause(SearchClauseInterface $searchClause, Query $query)
    {
 
        $indexName = 'ses_product_ses_datamap_priority_value_i';
 
        $direction = $searchClause->getDirection() === AbstractSortCriterion::DESC
            ? Query::SORT_DESC
            : Query::SORT_ASC;
 
        $query->addSort($indexName, $direction);
    }
 
    /**
     * @param SearchClauseInterface $searchClause
     * @return bool
     */
    public function canHandle(SearchClauseInterface $searchClause)
    {
        return $searchClause instanceof PriorityFieldSorting;
    }
}
```

### 3. Add configuration in services.xml

In parameters you should place the namespace of the handler class.

Don't forget to add the tag name to the service definition.

This will register handler and it will be used whenever search clause is an instance of PriorityFieldSorting.

This means that when you add this to eshopQuery it will executre the handler

```
<parameter key="siso_search.search_sort_handler.priority.econtent.class">MyProject\Bundle\ProjectBundle\Service\Search\PriorityFieldSortingHandler</parameter>
  
<service id="siso_search.search_sort_handler.priority.econtent.class" class="%siso_search.search_sort_handler.priority.econtent.class%">
    <tag name="siso_search.search_clause_handler" type="econtent" />
</service>
```

### 4. Use it!

``` php
$query = new EshopQuery();
  
// Insert here all the query options you like.
  
// This is our new sorting criteria.
$query->setSortCriteria(
    array(
        new PriorityFieldSorting(
            array(
                'direction' => 'desc'
            )
        ),
    )
);
```

## Please note that a query could have several sorting criteria

``` php
$eshopQuery->setSortCriteria(
    array(
        new MyCustomSorting1(array('direction' => 'asc')),
        new MyCustomSorting2(array('direction' => 'desc')),
        new MyCustomSorting3(array('direction' => 'desc')),
        ...
    )
```

The order of the sorting criteria is important. In the example above the sorting will be in the order they are set into the array.

!!! note "Modifying existing SortHandlers"

    Existing SortHandlers can't be overriden by simply extending them. If you need to change the behavior of a SortHandler, you need to use the new implementation explicitly, i.e. instantiate the new class like shown in the example above.
