#  Breadcrumbs - Cookbook 

# How to configure breadcrumbs for your custom route?

Let's assume that you created a new custom action in your controller.

Example: 

BlogController::indexAction in your custom AppBundle (with the code below)

``` 
class BlogController extends BaseController
{
    public function indexAction(Request $request)
    {
        //some action
    }
}
```

Afterwards you created a route for this action in routing.yml

``` 
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
```

In order to set custom breadcrumbs for this route we need to add some configuration to this route.

``` 
 custom_blog_index:
     pattern:  /blog/index
     defaults:
        _controller: AppBundle:Blog:index
+        breadcrumb_path: custom_blog_index
+        breadcrumb_names: Blog List
```

<table>
<thead>
<tr class="header">
<th>Option</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>breadcrumb_path</td>
<td>It needs to be a valid route name, which exists in at least one of the routing yml files</td>
</tr>
<tr>
<td>breadcrumb_names</td>
<td><p>It is a key for the translation. This name that will be shown in breadcrumbs. If the translation is not set there will be a fallback to route translation.</p>
<p>In the example above if the "Blog List'' key has no translation then the fallback key would be "custom_blog_index|breadcrumb"</p></td>
</tr>
</tbody>
</table>

#### More advanced usage of routes

If you want breadcrumbs to have more items than just one, then you can specify more paths and names with "/" delimiter.

In the example below when breadcrumbs will be generated there will be 2 elements (Profile and Blog list)

``` 
 custom_blog_index:
     pattern:  /blog/index
     defaults:
         _controller: AppBundle:Blog:index
+        breadcrumb_path: silversolutionsCustomerDetail/custom_blog_index
+        breadcrumb_names: Profile/Blog List
```

The is a limitation, when using this routes generator. You can provide only the paths that the corresponding action in the controller have no parameters required \! There is no way of passing any parameters to the action.

# How to write your own breadcrumbs generator?

#### Step 1

First we need to write a generator class. It needs to implement the "BreadcrumbsGeneratorInterface" that have only 2 methods.

There is an abstract class, which provides access to the WhiteOctober breadcrumbs library. It is used in this example. For the case that the BreadcrumbsGeneratorInterface implementation, for some reason, can't or just isn't extending the AbstractWhiteOctoberBreadcrumbsGenerator, the method renderBreadcrumbs() must completely take care of rendering the HTML code for the breadcrumbs.

``` 
<?php
/**
 * Product silver.e-shop
 *
 * A powerful e-commerce solution for B2B online shops / portals and complex
 * online applications that have access to ERP data, usually in real time.
 * http://www.silversolutions.de/eng/Products/silver.e-shop
 *
 * This file contains the class CatalogBreadcrumbsGenerator
 *
 * @copyright Copyright (C) 2013 silver.solutions GmbH. All rights reserved.
 * @license see vendor/silversolutions/silver.e-shop/license_txt_ger.pdf
 * @version $Version$
 * @package silver.e-shop
 */

namespace Path\To\Your\Generator\Breadcrumbs;

use Symfony\Component\HttpFoundation\Request;
use Silversolutions\Bundle\EshopBundle\Service\Breadcrumbs\AbstractWhiteOctoberBreadcrumbsGenerator;

class CustomBreadcrumbsGenerator extends AbstractWhiteOctoberBreadcrumbsGenerator
{
    /**
     * @var CustomService
     */
    private $customService:

    public function setCustomService(CustomService $customService)
    {
        $this->customService = $customService;
    }

    /**
     * Verification method for the generator's operational responsibility
     *
     * Determines if the implementation instance is able to render the
     * breadcrumbs for a specific request.
     *
     * @param Request $request
     * @return bool
     */
    public function canRenderBreadcrumbs(Request $request)
    {
        // custom logic, for example:
        return $request->attributes->has('custom_attribute');
    }

    /**
     * Renders the breadcrumbs for the current request.
     *
     * @param Request $request
     * @return string Rendered mark-up content for breadcrumbs
     */
    public function renderBreadcrumbs(Request $request)
    {
        try {
            // custom logic, for example
            $elements = $this->customService->getElements($request->attributes->get('custom_attribute'));

            foreach ($elements as $element) {
                $this->woBreadcrumbsModel->prependItem($element->getName(), $element->getUrl());
            }
        } catch (\Exception $e) {
            //don't do anything, no breadcrumb will be generated
        }

        return $this->renderWhiteOctoberBreadcrumbsModel();
    }
}
```

#### Step 2

Add service definition and tag it properly as a breadcrumbs generator with a respective priority. The highest priority which matches with canRender() will render the breadcrumbs for the current request.

``` 
<parameter key="siso_core.breadcrumbs_generator.custom.class">Path\To\Your\Generator\CustomBreadcrumbsGenerator</parameter>

<!-- custom breadcrumbs generator-->
<service id="siso_core.breadcrumbs_generator.custom"
         class="%siso_core.breadcrumbs_generator.custom.class%"
         parent="siso_core.abstract_breadcrumbs_generator.white_october">
    <call method="setCustomService">
        <argument type="service" id="custom_service" />
    </call>
    <tag name="siso_core.breadcrumbs_generator" priority="120" />
</service>
```

# Additional data in translationParameters

translationParameters contains additional data which can be used to change the behavior of the breadcrumbs depending on the stored data. **Every** BreadcrumbsGenerator has to add an array with type, identifier and content\_type\_id. Always create all the keys and leave the elements empty if not needed. Examples from some BreadcrumbsGenerators:

<table>
<thead>
<tr class="header">
<th>Silversolutions/Bundle/EshopBundle/Service/Breadcrumbs/EzContentBreadcrumbsGenerator.php</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>array(
 &#39;type&#39; =&gt; self::EZ_CONTENT_BREADCRUMB_TYPE,
 &#39;identifier&#39; =&gt; $content-&gt;id,
 &#39;content_type_id&#39; =&gt; $location-&gt;contentInfo-&gt;contentTypeId
)</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&#39;TYPE&#39; =&gt; STRING &#39;EZ_CONTENT&#39; (LENGTH=10)
&#39;IDENTIFIER&#39; =&gt; INT 57
&#39;CONTENT_TYPE_ID&#39; =&gt; INT 23</code></pre>
</td>
</tr>
<tr>
<td>Silversolutions/Bundle/EshopBundle/Service/Breadcrumbs/CatalogBreadcrumbsGenerator.php</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>array(
 &#39;type&#39; =&gt; self::CATALOG_BREADCRUMB_TYPE,
 &#39;identifier&#39; =&gt; $catalogIdentifier,
 &#39;content_type_id&#39; =&gt; &#39;&#39;
)</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&#39;TYPE&#39; =&gt; STRING &#39;CATALOG&#39; (LENGTH=7)
&#39;IDENTIFIER&#39; =&gt; STRING &#39;9686&#39; (LENGTH=4)
&#39;CONTENT_TYPE_ID&#39; =&gt; STRING &#39;&#39; (LENGTH=0)</code></pre>
</td>
</tr>
<tr>
<td><p>Silversolutions/Bundle/EshopBundle/Service/Breadcrumbs/RoutesBreadcrumbsGenerator.php</p></td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>array(
 &#39;type&#39; =&gt; self::ROUTE_BREADCRUMB_TYPE,
 &#39;identifier&#39; =&gt; &#39;&#39;,
 &#39;content_type_id&#39; =&gt; &#39;&#39;
)</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&#39;TYPE&#39; =&gt; STRING &#39;ROUTE&#39; (LENGTH=5)
&#39;IDENTIFIER&#39; =&gt; STRING &#39;&#39; (LENGTH=0)
&#39;CONTENT_TYPE_ID&#39; =&gt; STRING &#39;&#39; (LENGTH=0)</code></pre>
</td>
</tr>
</tbody>
</table>
