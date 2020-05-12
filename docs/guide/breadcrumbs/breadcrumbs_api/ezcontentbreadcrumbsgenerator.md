# EzContentBreadcrumbsGenerator

`Silversolutions\\Bundle\\EshopBundle\\Service\\Breadcrumbs\\EzContentBreadcrumbsGenerator`
renders the breadcrumbs for standard eZ Platform content.  

## When it is used

The generator triggers if the session attribute `location` is set. This is set by an eZ routine is the router matches a Content item.

## Rendering process

Renders all elements of the path of the currently displayed Location as breadcrumbs, up to the content root.
