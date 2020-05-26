# Modifying the search query

You can extend the search query before navigation data is fetched
using two events that are thrown before the search query is submitted:

- `PostBuildEzContentQueryEven`
- `PostBuildSolrQueryEvent`

## PostBuildEzContentQueryEvent

The `PostBuildEzContentQueryEvent` event is thrown just before content model content for the navigation is fetched.
You can listen to it using the following example listener:

``` php
<?php

namespace Siso\Bundle\SearchBundle\EventListener;

use Siso\Bundle\SearchBundle\Event\PostBuildEzContentQueryEvent;

class PostBuildEzContentQueryListener
{
    /**
     * Adds the criteria to the query
     *
     * @param PostBuildEzContentQueryEvent $event
     * @return void
     *
     */
    public function postBuildEzContentQuery(PostBuildEzContentQueryEvent $event)
    {
        $query = $event->getLocationQuery();
        $query->limit = NavigationHelper::QUERY_LIMIT;
    }
}
```

``` xml
<service id="siso_search.post_build_ez_content_query_listener" class="Siso\Bundle\SearchBundle\EventListener\PostBuildEzContentQueryListener">
    <tag name="kernel.event_listener" event="siso_search.post_build_ez_content_query" method="postBuildEzContentQuery" />
</service>
```

## PostBuildSolrQueryEvent

The `PostBuildSolrQueryEvent` event is thrown just before catalog content for the navigation is fetched.

If you are implementing a new data provider, you have to implement a listener to handle at least the languages.

``` php
<?php

namespace Siso\Bundle\SearchBundle\EventListener;

use eZ\Publish\Core\Repository\PermissionsCriterionHandler;
use EzSystems\EzPlatformSolrSearchEngine\Query\CriterionVisitor;
use Siso\Bundle\SearchBundle\Event\PostBuildSolrQueryEvent;
use Siso\Bundle\SearchBundle\PostBuildSolrQueryListenerInterface;

class EzPostBuildSolrQueryListener implements PostBuildSolrQueryListenerInterface
{
    /**
     * @var PermissionsCriterionHandler
     */
    protected $permissionsCriterionHandler;

    /**
     * @var CriterionVisitor
     */
    protected $criterionVisitor;

    /**
     * @var array used to get language to perform the search
     */
    protected $languages;

    /**
     * @param PermissionsCriterionHandler $permissionsCriterionHandler
     * @param CriterionVisitor $criterionVisitor
     * @param array $languages
     */
    public function __construct(
        PermissionsCriterionHandler $permissionsCriterionHandler,
        CriterionVisitor $criterionVisitor,
        array $languages
    ) {
        $this->permissionsCriterionHandler = $permissionsCriterionHandler;
        $this->criterionVisitor = $criterionVisitor;
        $this->languages = $languages;
    }

    /**
     * Adds the language filter and the permission criterion filter
     *
     * @param PostBuildSolrQueryEvent $event
     * @return void
     *
     */
    public function postBuildSolrQuery(PostBuildSolrQueryEvent $event)
    {
        $selectQuery = $event->getSolrQuery();

        //language filter
        $selectQuery->createFilterQuery('language')
            ->setQuery('always_available_b:true OR meta_indexed_language_code_s:(' . implode(' ', $this->languages) . ')');

        //permission criterion filter
        $permissionsCriterion = $this->permissionsCriterionHandler->getPermissionsCriterion('content');
        $selectQuery
            ->createFilterQuery('ez_permissions_criterion')
            ->setQuery(
                $this->criterionVisitor->visit($permissionsCriterion));

    }
}
```

``` xml
<service id="siso_search.ez_post_build_solr_query_listener" class="Siso\Bundle\SearchBundle\EventListener\EzPostBuildSolrQueryListener">
    <argument type="service" id="ezpublish.api.service.permissions_criterion_handler" />
    <argument type="service" id="ezpublish.search.solr.query.content.criterion_visitor.aggregate" />
    <argument>$languages$</argument>
</service>
```

This service is not tagged as a listener yet.
This happens with the `SolariumClientInjectionPass` Composer pass,
because there is no need for all listeners to be active at the same time (this depends on the data provider).
