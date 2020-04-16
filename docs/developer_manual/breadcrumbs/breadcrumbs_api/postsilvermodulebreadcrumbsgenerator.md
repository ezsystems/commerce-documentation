# PostSilverModuleBreadcrumbsGenerator

This breadcrumbs generator is for requests after a silver.module, which process the controller of the previous silver.module.

**Example**: A silver.module eZ content object specifies the controller SisoCustomerCenterBundle:Form:form in order to display a form. After this form is submitted, it will not request the URL of the silver.module, but the URL of the controller, defined by its route. But nonetheless, the breadcrumbs of the former silver.module shall be displayed. 

This generator is responsible for this special case of requests. It listens to two kernel events in order to add some request attributes and save the last used silver.module ID temporarily in the session.  
Hint

silver.modules themselves are standard eZ content and their breadcrumbs are handled solely by the parent of this class which is [EzContentBreadcrumbsGenerator](EzContentBreadcrumbsGenerator_23560916.html).

## FQN

Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator

## Criterion for responsibility

The generator triggers if the session attribute 'silverModuleLocation' is set. This is set by an event listener callback, which is described below.

## Notes to the rendering process

In order to memorize a formerly displayed silver.module element, its ID is stored in the session and restored if the next page matches the controller, which is defined in the respective silver.module. This is achieved via two kernel event listeners:

### Event: kernel.controller

Method: Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator::onKernelRequest()

Listener method for the preparation of the request for a post silver.module controller.

It checks the session for a formerly stored silver.module ID and whether the content object defines the controller, that matched in the current request. If the controllers doesn't match, the current request is just a call of an unrelated page and can be ignored. In the case it does match, the request attributes silverModule and silverModuleLocation are added for the rendering of the breadcrumbs.

### Event: kernel.response

Method: Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator::onKernelResponse()

Listener method the temporary storage of the silver.module ID in the session.

If the request attribute silverModule is set and defines an eZ content object, it is assumed that we are in a silver.module context. In this case the silver.module ID must be stored in the session for later requests.

If the attribute is not defined, the session value must be removed in order to not match this generator accidentally, when the controller of the silver.module matches outside of it's context.
