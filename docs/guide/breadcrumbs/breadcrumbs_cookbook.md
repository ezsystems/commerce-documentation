# Breadcrumbs cookbook

## Configuring breadcrumbs for custom routes

To configure breadcrumbs for your custom route you need to add configuration to this route.

``` yaml hl_lines="5 6"
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index
        breadcrumb_names: Blog List
```

The example above shows how to configure a custom route for the `BlogController::indexAction` action in your controller in `routing.yml`.

|Option|Description|
|--- |--- |
|breadcrumb_path|Needs to be a valid route name which exists in at least one of the routing YAML files|
|breadcrumb_names|Key for the translation. This name that will be shown in breadcrumbs. If the translation is not set, there will be a fallback to route translation.</br>In the example above if the `Blog List` key has no translation, then the fallback key would be `custom_blog_index\|breadcrumb`|

#### More advanced usage of routes

If you want breadcrumbs to have more items than just one, you can specify more paths and names with the `/` delimiter.

In the example below when breadcrumbs are generated there will be two elements (Profile and Blog list):

``` yaml hl_lines="5 6"
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: silversolutionsCustomerDetail/custom_blog_index
        breadcrumb_names: Profile/Blog List
```

!!! caution

    When using this route generator you can provide paths only if the corresponding action in the controller has no required parameters. There is no way of passing any parameters to the action.

## Creating a breadcrumb generator

### Create a generator class

First, write a generator class.
It needs to implement `BreadcrumbsGeneratorInterface` that has two methods.

!!! note

    There is an abstract class, which provides access to the WhiteOctober breadcrumbs library. It is used in this example. For the case that the BreadcrumbsGeneratorInterface implementation, for some reason, can't or just isn't extending the AbstractWhiteOctoberBreadcrumbsGenerator, the method renderBreadcrumbs() must completely take care of rendering the HTML code for the breadcrumbs.

``` php
<?php

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

### Add service definition

Add service definition and tag it properly as a breadcrumbs generator with a respective priority.
The highest priority which matches `canRender()` will render the breadcrumbs for the current request.

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

## Additional data in `translationParameters`

`translationParameters` contains additional data which can be used to change the behavior of the breadcrumbs depending on the stored data.
Every `BreadcrumbsGenerator` has to add an array with `type`, `identifier` and `content_type_id`.
Always create all the keys and leave the elements empty if not needed. Examples from `BreadcrumbsGenerators`:

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
