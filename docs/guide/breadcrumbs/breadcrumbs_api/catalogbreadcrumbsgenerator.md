# CatalogBreadcrumbsGenerator

`Silversolutions\\Bundle\\EshopBundle\\Service\\Breadcrumbs\\CatalogBreadcrumbsGenerator`
renders the breadcrumbs of all catalog elements regardless of the data source of the catalog.

## When it is used

Checks if the `url` attribute is set in the request by the `StandardRouter`. If so, the catalog element matching the request and breadcrumb can be rendered.

## Rendering process

Uses the `CatalogDataProvider` to fetch all catalog parent elements up to the catalog root element for the last half of the breadcrumbs. And as the catalog root is part of the content tree, it fetches the parent Locations of this element as the first half of the breadcrumbs and prepends them.
