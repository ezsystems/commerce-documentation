#  How to implement a custom sorting handler for econtent 

## In this example we will create a custom sorting that will use priority field.

This will create a new sort criteria for use with Solr. You will be able to specify the Solr field name for sort. In this example it will be a field that stores product priority, but it can be any Solr field you want.

## Steps

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><ol>
<li>Create a new class that extends AbstractSortCriterion</li>
</ol></td>
<td>

<pre class="" data-syntaxhighlighter-params="brush: php; gutter: false; theme: DJango" data-theme="DJango"><code>&lt;?php

namespace MyProject\Bundle\ProjectBundle\Api\Search;

use Siso\Bundle\SearchBundle\Api\AbstractSortCriterion;

class PriorityFieldSorting extends AbstractSortCriterion
{
}</code></pre>
<p> </p>
<div>
<p> </p>
</td>
</tr>
<tr>
<td><p>2. Create new handler class.</p>
<p>Please note that variable $indexName is the Solr field that will be used to sort.</p>
<p>You can choose any field you like.</p></td>
<td><div>
<pre class="" data-syntaxhighlighter-params="brush: php; gutter: false; theme: DJango" data-theme="DJango"><code>&lt;?php

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

        $indexName = &#39;ses_product_ses_datamap_priority_value_i&#39;;

        $direction = $searchClause-&gt;getDirection() === AbstractSortCriterion::DESC
            ? Query::SORT_DESC
            : Query::SORT_ASC;

        $query-&gt;addSort($indexName, $direction);
    }

    /**
     * @param SearchClauseInterface $searchClause
     * @return bool
     */
    public function canHandle(SearchClauseInterface $searchClause)
    {
        return $searchClause instanceof PriorityFieldSorting;
    }
}</code></pre>
<p> </p>
<p> </p>
</td>
</tr>
<tr>
<td><p> 3. Add configuration in services.xml</p>
<p>In parameters you should place the namespace of the handler class.</p>
<p>Don't forget to add the tag name to the service definition.</p>
<p> </p></td>
<td><p>This will register handler and it will be used whenever search clause is an instance of PriorityFieldSorting.</p>
<p>This means that when you add this to eshopQuery it will executre the handler</p>
<p> </p>
<pre class="" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: DJango" data-theme="DJango"><code>&lt;parameter key=&quot;siso_search.search_sort_handler.priority.econtent.class&quot;&gt;MyProject\Bundle\ProjectBundle\Service\Search\PriorityFieldSortingHandler&lt;/parameter&gt;
 
&lt;service id=&quot;siso_search.search_sort_handler.priority.econtent.class&quot; class=&quot;%siso_search.search_sort_handler.priority.econtent.class%&quot;&gt;
    &lt;tag name=&quot;siso_search.search_clause_handler&quot; type=&quot;econtent&quot; /&gt;
&lt;/service&gt;</code></pre>
<p> </p>
<div>
<p> </p>
</td>
</tr>
<tr>
<td>4. Use it!</td>
<td>

<pre class="" data-syntaxhighlighter-params="brush: php; gutter: false; theme: DJango" data-theme="DJango"><code>$query = new EshopQuery();
 
// Insert here all the query options you like.
 
// This is our new sorting criteria.
$query-&gt;setSortCriteria(
    array(
        new PriorityFieldSorting(
            array(
                &#39;direction&#39; =&gt; &#39;desc&#39;
            )
        ),
    )
);
 </code></pre>

</td>
</tr>
</tbody>
</table>

### Please note that a query could have several sorting criteria

``` 
$eshopQuery->setSortCriteria(
    array(
        new MyCustomSorting1(array('direction' => 'asc')),
        new MyCustomSorting2(array('direction' => 'desc')),
        new MyCustomSorting3(array('direction' => 'desc')),
        ...
    )
```

The order of the sorting criteria is important. In the example above the sorting will be in the order they are set into the array.
