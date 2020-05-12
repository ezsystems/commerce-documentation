# Ajax (Phalanx)

![](img/ajax_1.png)

## Workflow

!!! note "Phalanx"

    For a detailed usage of Phalanx, please see: <frontend_customizing/4_front_end_stack_in_details/4.4_phalanx/4.4.1_how_to_create_hoplite/4.4.1_how_to_create_a_hoplite.md>

    For the detailed information about how to create a hoplite, see: [How to create a hoplite](frontend_customizing/4_front_end_stack_in_details/4.4_phalanx/4.4_phalanx.md).

- Ajax requests use POST or GET method (configured in hoplites):
  - GET should be used for most requests except of sensitive data. GET requests can be cached.
  - POST should be used to send sensitive information. POST requests are never cached.
- All ajax requests go the same URL:

``` 
silversolutions_phalanx:
    path: /_ajax_/phalanx
    defaults:
        _controller: SilversolutionsEshopBundle:Ajax:phalanx
```

- The AjaxController (see above) then decides, which AjaxSubController should the request be forwarded to. This depends on the parameters, that needs to be send from the hoplite.  
  - 'type' - defines which SubController will be called
  - key of the method - defines which method in the SubController will be called. The key always must follow these rules: lowercase letters separated by underscore.

**How the parameters are sent from hoplite**

``` 
var data = [];
// 'type' needs to be send in every call inside of data and determines the SubController
data[0] = {
  'form': $(this).parents('.js-product-line').find('input, select').serialize(),
  'type': 'basket'
};
// key of the method: 'add_basket' determines the method of the SubController
requestCollection['add_basket'].add(data);

// this will execute following controller: AjaxBasketController::addBasketAction(Request $request, array $data = array())
// see explanation below
```

**How the SubController and the method are determined in backend**

``` 
//Every SubController will follow this syntax
$type = 'basket';
$subController = 'Ajax' . ucfirst(strtolower($type)) . 'Controller'; 
//from the example above it means
$subController = 'AjaxBasketController';

//Every SubController method will follow this syntax
$keyOfTheMethod = 'add_basket';
$method = $this->underscoreToCamelcase($keyOfTheMethod) . 'Action';
//from the example above it means
$method = 'addBasketAction';
```

- Every SubController method must follow these rules:
  - Every SubController must extend Silversolutions\\Bundle\\EshopBundle\\Controller\\**BaseController**
  - Every SubController method excepts following paramaters: *Request $request, array $data = array()*
  - Every SubController method must return AjaxSubResponse() to the AjaxController  

``` 
use Silversolutions\Bundle\EshopBundle\Http\AjaxSubResponse;
use Silversolutions\Bundle\EshopBundle\Controller\BaseController;

class AjaxBasketController extends BaseController
{
    public function addBasketAction(Request $request, array $data = array())
    {
        $response = new AjaxSubResponse();   

        $responseContent = [
            'data' => 'some text data',        
        ];
        $response->setContentData($responseContent);

        return $response;
    }
}
```

- AjaxController returns all data in json format.

### How to override AjaxSubControllers

#### Override the SubController

1.  Create a new controller class, which extends the controller that will be overridden.
    - Example: `Silversolutions\Bundle\ProjectBundle\Controller\AjaxProjectBasketController` extends `Silversolutions\Bundle\EshopBundle\Controller\AjaxBasketController`
2.  Implement a new action method or reimplement a parent method.
3.  You have two possibilities to override the default logic and influence which *SubController* will be called:  
    1.  Either you make sure, that your bundle is loaded in Kernel after the affected silver.e-shop bundle
    2.  Or you create a parameter that defines the new controller, just make sure to keep the correct syntax

    ``` 
    # following syntax must be kept:
    # siso_eshop.ajax_controller.<type>
    siso_eshop.ajax_controller.basket: SilversolutionsProjectBundle:AjaxProjectBasket
    ```

### HTTP caching of ajax requests

A response of GET requests could be cached. To cache it you should [use HttpCachingServiceStrategy](../guide/cache/content_cache_refresh/http_caching.md) in your Ajax\<Type\>Controller.

!!! caution

    AjaxController copies **all http headers** from `Ajax<Type>Controller` subresponse to the final response. It is done to pass caching headers to reverse proxy.
