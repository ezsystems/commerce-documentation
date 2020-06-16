# Search API

![](../img/search_6.png)

## Search interfaces

The following interfaces represent the entry point for search:

- `Siso\Bundle\SearchBundle\Api\EshopContentSearchInterface`
- `Siso\Bundle\SearchBundle\Api\EshopCatalogSearchInterface`
- `Siso\Bundle\SearchBundle\Api\EshopProductSearchInterface`

### EshopQuery

`Siso\Bundle\SearchBundle\Api\EshopQuery` is the value object class for all search query parameters.

### SearchContext

The current controller implementation for the eshop search uses a service call to instantiate the `SearchContext`: `\Siso\Bundle\SearchBundle\Service\SearchContextService`.
It has the service ID: `siso_search.search_context_service.default`.
To change the default implementation, override this service.

The search context defines context information for the query which is not contained in the search clauses.
This can be, for example, data related to the SiteAccess (which was addressed by the HTTP request).
This information can be used by the respective search service implementation.

### SearchContextServiceInterface

Implementations of `SearchContextServiceInterface` are used by the standard search controller to get the instance of the `SearchContext`.

### SearchClauseInterface

The `SearchClauseInterface` marker interface has no methods. It's used for `instanceof` checks and type hinting for all sub-interfaces or sub-classes.

### ConditionInterface

`ConditionInterface` marker interface has no methods.

### SortCriterionInterface

`SortCriterionInterface` is the basic interface for all sort criteria.

### FacetInterface

`FacetInterface` is a setter/getter interface for facets.

### FacetOptionInterface

`FacetOptionInterface` is a setter/getter interface for facet options.

### FacetServiceInterface

The handling of facets can complicate controller and template code.
The `FacetServiceInterface` interface collects complex correlations of the request and response processes of facets in one class.
It is recommended to use only this interface for facet handling and create new implementations of it,
if the current standard is not sufficient.

### SearchClauseHandlerInterface

`SearchClauseHandlerInterface` is the basic search clause handler interface.

### EzSearchClauseHandlerInterface

`EzSearchClauseHandlerInterface` is used for specific implementations which use the eZ Platform Search API for queries.

### SortServiceInterface

`SortServiceInterface` defines methods for setting the correct sort criteria for the search query.

It is recommended to use only this interface for sorting and create new implementations of it,
if the current standard is not sufficient.
