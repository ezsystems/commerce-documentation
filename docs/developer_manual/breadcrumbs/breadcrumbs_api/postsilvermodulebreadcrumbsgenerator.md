#  PostSilverModuleBreadcrumbsGenerator 

This breadcrumbs generator is for requests after a silver.module, which process the controller of the previous silver.module.

**Example**: A silver.module eZ content object specifies the controller SisoCustomerCenterBundle:Form:form in order to display a form. After this form is submitted, it will not request the URL of the silver.module, but the URL of the controller, defined by its route. But nonetheless, the breadcrumbs of the former silver.module shall be displayed. 

This generator is responsible for this special case of requests. It listens to two kernel events in order to add some request attributes and save the last used silver.module ID temporarily in the session.  
Hint

silver.modules themselves are standard eZ content and their breadcrumbs are handled solely by the parent of this class which is [EzContentBreadcrumbsGenerator](EzContentBreadcrumbsGenerator_23560916.html).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td>FQN</td>
<td>Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator</td>
</tr>
<tr>
<td>Criterion for responsibility</td>
<td>The generator triggers if the session attribute 'silverModuleLocation' is set. This is set by an event listener callback, which is described below.</td>
</tr>
<tr>
<td>Notes to the rendering process</td>
<td><p>In order to memorize a formerly displayed silver.module element, its ID is stored in the session and restored if the next page matches the controller, which is defined in the respective silver.module. This is achieved via two kernel event listeners:</p>
<div class="panel" style="border-style: dashed;border-width: 1px;">
<div class="panelHeader" style="border-bottom-width: 1px;border-bottom-style: dashed;">
<strong>Event: kernel.controller</strong>

<p><strong>Method</strong>: Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator::onKernelRequest()</p>
<p>Listener method for the preparation of the request for a post silver.module controller.</p>
<p>It checks the session for a formerly stored silver.module ID and whether the content object defines the controller, that matched in the current request. If the controllers doesn't match, the current request is just a call of an unrelated page and can be ignored. In the case it does match, the request attributes silverModule and silverModuleLocation are added for the rendering of the breadcrumbs.</p>
<div class="panel" style="border-style: dashed;border-width: 1px;">
<div class="panelHeader" style="border-bottom-width: 1px;border-bottom-style: dashed;">
<strong>Event: kernel.response</strong>

<p><strong>Method</strong>: Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\PostSilverModuleBreadcrumbsGenerator::onKernelResponse()</p>
<p>Listener method the temporary storage of the silver.module ID in the session.</p>
<p>If the request attribute silverModule is set and defines an eZ content object, it is assumed that we are in a silver.module context. In this case the silver.module ID must be stored in the session for later requests.</p>
<p>If the attribute is not defined, the session value must be removed in order to not match this generator accidentally, when the controller of the silver.module matches outside of it's context.</p>

</td>
</tr>
</tbody>
</table>
