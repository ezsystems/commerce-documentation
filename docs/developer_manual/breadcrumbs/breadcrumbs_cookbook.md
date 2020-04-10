# Breadcrumbs cookbook

## How to configure breadcrumbs for your custom route?

Let's assume that you created a new custom action in your controller.

Example: 

BlogController::indexAction in your custom AppBundle (with the code below)

``` php
class BlogController extends BaseController
{
    public function indexAction(Request $request)
    {
        //some action
    }
}
```

Afterwards you created a route for this action in routing.yml

``` yaml
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
```

In order to set custom breadcrumbs for this route we need to add some configuration to this route.

``` yaml
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
+        breadcrumb_path: custom_blog_index
+        breadcrumb_names: Blog List
```

|Option|Description|
|--- |--- |
|breadcrumb_path|It needs to be a valid route name, which exists in at least one of the routing yml files|
|breadcrumb_names|It is a key for the translation. This name that will be shown in breadcrumbs. If the translation is not set there will be a fallback to route translation.</br>In the example above if the "Blog List'' key has no translation then the fallback key would be "custom_blog_index\|breadcrumb"|

#### More advanced usage of routes

If you want breadcrumbs to have more items than just one, then you can specify more paths and names with "/" delimiter.

In the example below when breadcrumbs will be generated there will be 2 elements (Profile and Blog list)

``` yaml
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
+        breadcrumb_path: silversolutionsCustomerDetail/custom_blog_index
+        breadcrumb_names: Profile/Blog List
```

!!! caution

    There is a limitation, when using this routes generator. You can provide only the paths that the corresponding action in the controller have no parameters required. There is no way of passing any parameters to the action.

## How to write your own breadcrumbs generator?

#### Step 1

First we need to write a generator class. It needs to implement the "BreadcrumbsGeneratorInterface" that have only 2 methods.

!!! note

    There is an abstract class, which provides access to the WhiteOctober breadcrumbs library. It is used in this example. For the case that the BreadcrumbsGeneratorInterface implementation, for some reason, can't or just isn't extending the AbstractWhiteOctoberBreadcrumbsGenerator, the method renderBreadcrumbs() must completely take care of rendering the HTML code for the breadcrumbs.

``` php
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

## Additional data in translationParameters

translationParameters contains additional data which can be used to change the behavior of the breadcrumbs depending on the stored data. **Every** BreadcrumbsGenerator has to add an array with type, identifier and content\_type\_id. Always create all the keys and leave the elements empty if not needed. Examples from some BreadcrumbsGenerators:

### EzContentBreadcrumbsGenerator

`Silversolutions/Bundle/EshopBundle/Service/Breadcrumbs/EzContentBreadcrumbsGenerator.php`

``` php
array(
 'type' => self::EZ_CONTENT_BREADCRUMB_TYPE,
 'identifier' => $content->id,
 'content_type_id' => $location->contentInfo->contentTypeId
)
```

Example:

``` php
'TYPE' => STRING 'EZ_CONTENT' (LENGTH=10)
'IDENTIFIER' => INT 57
'CONTENT_TYPE_ID' => INT 23
```

### CatalogBreadcrumbsGenerator

`Silversolutions/Bundle/EshopBundle/Service/Breadcrumbs/CatalogBreadcrumbsGenerator.php`

``` php
array(
 'type' => self::CATALOG_BREADCRUMB_TYPE,
 'identifier' => $catalogIdentifier,
 'content_type_id' => ''
)
```

Example:

``` php
'TYPE' => STRING 'CATALOG' (LENGTH=7)
'IDENTIFIER' => STRING '9686' (LENGTH=4)
'CONTENT_TYPE_ID' => STRING '' (LENGTH=0)
```

### RoutesBreadcrumbsGenerator

`Silversolutions/Bundle/EshopBundle/Service/Breadcrumbs/RoutesBreadcrumbsGenerator.php`

``` php
array(
 'type' => self::ROUTE_BREADCRUMB_TYPE,
 'identifier' => '',
 'content_type_id' => ''
)
```

Example:

``` php
'TYPE' => STRING 'ROUTE' (LENGTH=5)
'IDENTIFIER' => STRING '' (LENGTH=0)
'CONTENT_TYPE_ID' => STRING '' (LENGTH=0)
```
