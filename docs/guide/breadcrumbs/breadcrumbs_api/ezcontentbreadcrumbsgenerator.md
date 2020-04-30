# EzContentBreadcrumbsGenerator

`Silversolutions\\Bundle\\EshopBundle\\Service\\Breadcrumbs\\EzContentBreadcrumbsGenerator`
the breadcrumbs for standard eZ Platform content.  

## When it is used

The generator triggers if the session attribute `location` is set. This is set by an eZ routine in the case the router matched a Content item.

## Rendering process

Renders all elements of the path of the currently displayed Location as breadcrumbs, up to the content root.
