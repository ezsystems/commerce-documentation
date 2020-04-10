# CatalogBreadcrumbsGenerator

Renders the breadcrumbs of all catalog elements regardless of the data source of the catalog.

## FQN

Silversolutions\\Bundle\\EshopBundle\\Service\\Breadcrumbs\\CatalogBreadcrumbsGenerator

## Criterion for responsibility

Checks if the 'url' attribute is set in the request by the StandardRouter. If so, then catalog element matched the request and breadcrumb can be rendered.

## Notes to the rendering process

Uses the [CatalogDataProvider](Term---Dataprovider_23560828.html) in order to fetch all catalog parent elements up to the catalog root element for the last later half of the breadcrumbs. And as the catalog root is part of the eZ content tree, it fetches the parent locations of this element as the first half of the breadcrumbs and prepends them.
