#  Navigation - Cookbook 

## How to influence the search query?

You have the possibility to extend the search query before navigation data is fetched. Before the search query is submitted, there are events that allows you to extend the query:

  - ``` 
    PostBuildEzContentQueryEvent
    ```

  - ``` 
    PostBuildSolrQueryEvent
    ```
## PostBuildEzContentQueryEvent

This event is thrown just before the eZ content for the navigation is fetched and it allows you to extend the search query. You have to implement an event listener, if you want to listen to it.

``` 
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

``` 
<service id="siso_search.post_build_ez_content_query_listener" class="Siso\Bundle\SearchBundle\EventListener\PostBuildEzContentQueryListener">
    <tag name="kernel.event_listener" event="siso_search.post_build_ez_content_query" method="postBuildEzContentQuery" />
</service>
```

## PostBuildSolrQueryEvent

This event is thrown just before the catalog content for the navigation is fetched and it allows you to extend the search query. You have to implement an event listener, if you want to listen to it.

If you are implementing a new dataprovider, you **have to** implement a listener to handle at least the languages.

``` 
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

``` 
<service id="siso_search.ez_post_build_solr_query_listener" class="Siso\Bundle\SearchBundle\EventListener\EzPostBuildSolrQueryListener">
    <argument type="service" id="ezpublish.api.service.permissions_criterion_handler" />
    <argument type="service" id="ezpublish.search.solr.query.content.criterion_visitor.aggregate" />
    <argument>$languages$</argument>
</service>
```

Notice that this service is not tagged as a listener yet\! This will be done via composer pass, since there is no need that all listeners are active at the same time (since depends on the dataprovider).

**Siso/Bundle/SearchBundle/DependencyInjection/Compiler/SolariumClientInjectionPass.php**

``` 
public function process(ContainerBuilder $container)
{
    $catalogDataProviderTarget = $container->getParameter('silver_eshop.default.catalog_data_provider');
    if ($catalogDataProviderTarget === 'econtent') {
        if (!$container->hasDefinition(self::SOLR_ECONTENT_CLIENT_ID)) {
            throw new \LogicException(
                'Catalog data provider is econtent, but service "'.self::SOLR_ECONTENT_CLIENT_ID
                .'" is not defined. Please check your configuration!');
        }
        ...
        $definition = $container->getDefinition('siso_search.econtent_post_build_solr_query_listener');
        $definition->addTag('kernel.event_listener',
            array('event' => 'siso_search.post_build_solr_query', 'method' => 'postBuildSolrQuery'));

    } else {        
        // Inject the default service as a fallback, if econtent is not used.   
        ...
        $definition = $container->getDefinition('siso_search.ez_post_build_solr_query_listener');
        $definition->addTag('kernel.event_listener',
            array('event' => 'siso_search.post_build_solr_query', 'method' => 'postBuildSolrQuery'));
    }
}
```
