# PostSilverModuleBreadcrumbsGenerator

`Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator`
handles requests after a `silver.module`, which process the controller of the previous `silver.module`.

**Example**: A silver.module Content item points to the controller `SisoCustomerCenterBundle:Form:form` to display a form. After this form is submitted, it will not request the URL of the silver.module, but the URL of the controller, defined by its route. The breadcrumbs of the former silver.module are still displayed. 

This generator handles such requests. It listens to two kernel events to add some request attributes and temporarily save ID of the last used silver.module in the session.

!!! tip

    silver.modules themselves are standard Content items and their breadcrumbs are handled solely by the parent of the [`EzContentBreadcrumbsGenerator`](ezcontentbreadcrumbsgenerator.md) class.

## When it is used

The generator triggers if the session attribute 'silverModuleLocation' is set. This is set by an event listener callback, which is described below.

## Rendering process

In order to memorize a formerly displayed silver.module element, its ID is stored in the session and restored if the next page matches the controller, which is defined in the respective silver.module. This is achieved via two kernel event listeners.

### kernel.controller event

`Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator::onKernelRequest()`
is the listener method that prepares the request for a post `silver.module` controller.

It checks the session for a formerly stored silver.module ID and whether the Content item defines the controller that matched in the current request. If the controller doesn't match, the current request is just a call of an unrelated page and can be ignored. If it does match, the request attributes `silverModule` and `silverModuleLocation` are added for the rendering of the breadcrumbs.

### kernel.response event

`Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator::onKernelResponse()`
is the listener method that temporarily stores the `silver.module` ID in the session.

If the request attribute silverModule is set and defines a Content item, it is assumed that you are in a silver.module context. In this case the silver.module ID must be stored in the session for later requests.

If the attribute is not defined, the session value must be removed in order to not match this generator accidentally, when the controller of the silver.module matches outside of its context.
