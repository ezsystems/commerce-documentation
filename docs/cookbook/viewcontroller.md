# ViewController

## Issue

Ez ViewController is throwing an exception, if content is not translated in language required in current siteaccess. This is happening in the template, such it is called as a subcontroller and if exception occurs, the page is broken.

``` html+twog
  {{ render(
                controller(
                'ez_content:viewLocation',
                {
                'locationId': item.locationId,
                'viewType': 'block_item',
                'params' : {'image_alias' : image_alias}
                }
                )
                ) 
  }}
```

## Solution

Ez ViewController is defined as a service, that is why eZ Commerce overrides the implementation to handle the exception. In case that exception occurs, no content will be returned, but the page will not be broken.

It is not necessary to change the templates, you can still go with the call of `ez_content:viewLocation`.

Additionally the exception will be logged.

## eZ Commerce ViewController

``` php
namespace Silversolutions\Bundle\EshopBundle\Controller;

use eZ\Publish\Core\MVC\Symfony\Controller\Content\ViewController as EzViewController;
use Symfony\Component\HttpFoundation\Response;

/**
 * This controller is responsible for viewing eZ Platform content
 */
class ViewController extends EzViewController
{
    /**
     * Overrides parent to be able to handle the returned exception.
     * In the case of any exception the location should not be rendered.
     *
     * @param int $locationId
     * @param string $viewType
     * @param bool $layout
     * @param array $params
     *
     * @return Response
     */
    public function viewLocation($locationId, $viewType, $layout = false, array $params = array())
    {
        try {
            $result = parent::viewLocation($locationId, $viewType, $layout, $params);
            return $result;

        } catch(\Exception $e) {
            $this->getLogger()->error(get_class($this) . ' catched Exception: "' . $e->getMessage());
            return new Response();
        }
    }
}
```

### Configuration

**ses_services.xml**

``` xml
<parameters>    
    <!-- override the eZ ViewController -->
    <parameter key="ezpublish.controller.content.view.class">Silversolutions\Bundle\EshopBundle\Controller\ViewController</parameter>
    <!-- override the eZ ViewController end -->
</parameters>
```
