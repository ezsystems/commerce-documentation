# How to modify the search query

Sometimes you need to modify the [EshopQuery](../search_api/search_api.md) just before it is send to the search service. This event listener will handle this situation. Maybe you need to modify the search term, or add some sorting criteria.

### Example

A use case can be searching for a book ISBN number, that needs to be modified.

User searches for 2-245-5-4878-0

You want to modify the search term to remove the - and search only for 224558780

## Usage

If you are using the SearchController, there is already an event that is dispatched after the complete query is builded. If you want to modify it, you need to implement an event listener for your project.

In the following example we are modifying the sort criteria of a query if the query is empty and if we detect that current sort criteria is Relevance Sorting (Which is the default one).

Please note that you can use any [EshopQuery](Search---API_23560412.html) getter methods to check any condition or property and then use any setter method to modify what ever you need.

``` php
use Siso\Bundle\SearchBundle\Api\Common\RelevanceSorting;
use Siso\Bundle\SearchBundle\Api\Common\SearchTermCondition;
use Siso\Bundle\SearchBundle\Event\PostBuildEshopQueryEvent;
use MyProject\Bundle\ProjectBundle\Api\Search\PriorityFieldSorting;

class EshopQueryListener
{
    /**
     * modifies the eshop query after it was build
     *
     * @param PostBuildEshopQueryEvent $event
     */
    public function onPostBuildEshopQuery(PostBuildEshopQueryEvent $event)
    {   
        /** all form parameters are stored here incl. the search context:
         *      - product
         *      - catalog
         *      - content
         */ 
        $params = $event->getParams();

        $eshopQuery = $event->getEshopQuery();
        $conditions = $eshopQuery->getConditions();

        foreach($conditions as $index => $condition) {
            if ($condition instanceof SearchTermCondition) {
                //if the search term is empty, sort additionally by the priority
                if ((($condition->searchTerm === '') || ($condition->searchTerm === '*')) &&
                    ($eshopQuery->getSortCriteria()[0] instanceof RelevanceSorting)) {
                        $eshopQuery->setSortCriteria(array(new PriorityFieldSorting(array('direction' => 'desc'))));
                }
            }
        }

        //modify the query by setting new conditions
        $eshopQuery->setConditions($conditions);
    }
} 
```

Service definition:

``` php
<parameter key="myproject.eshop_query_listener.class">MyProject\Bundle\ProjectBundle\EventListener\EshopQueryListener</parameter>

<service id="myproject.eshop_query_listener" class="%myproject.eshop_query_listener.class%">
    <tag name="kernel.event_listener" event="siso_search.post_build_eshop_query" method="onPostBuildEshopQuery" />
</service> 
```
