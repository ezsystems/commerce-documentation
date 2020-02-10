# Tutorial - Create a Hoplite 

This is complete guide on how to create a hoplite.

Let's assume we are creating an offer hoplite that can add a product to an offer (like add to basket)

# JavaScript Part

## Step 1: Create a hoplite file

You can create this file in custom bundle. Make sure to follow different approach for actions (storm.phalanx.actions.js)

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/phalanx/hoplite/offer/offer.js
```

### Example content

``` 
var hopliteOffer = phalanxController.extend({
    // constructor is triggered automatically when new instance of hoplite is created
    constructor: function() {
        var self = this;
        // extend defaults Controller settings
        this.defaults = _.extend({}, this.defaults, {
            callback_add_to_offer: 'callback_addToOffer', // callback name for add_basket TYPE
            name : 'offer' // type (hoplite) - make sure it's lowercase
        });
        Backbone.Model.apply(this, arguments);
        $(document).on('click', '.js-add-to-offer', function() {
          var btn = $(this);
          var data = {
            'sku' : btn.data('sku'),
            'quantity': 1
          };
          self.buildRequest('add_to_offer', data);
        });
    },
    /**
     * Builds Ajax request
     * @param action PHP action name
     * @param data collection of data
     * @method buildRequest
     */
    buildRequest: function (action, data) {
      data['type'] = 'offer'; // append to every request, required by PHP, type (hoplite) - make sure it's lowercase
      // create an object with special settings for the hoplite
      var requestCollection = {};
      requestCollection[action] = new Backbone.Collection;
      requestCollection[action].add(data);
      // create an instance of phalanxProvider and use it inside hoplite
      var ajax = new phalanxProvider;
      ajax.objHoplite = {};
      ajax.objHoplite[this.get('name')] = this;
      storm.spinner.show();
      ajax.call(requestCollection);
    },
    /**
     * Default callback for this hoplite.
     * This callback is NOT used when you specify to load a custom callback
     *
     * @param response {JSON} data received from backend
     */
    callback_addToOffer: function(response) {
        console.log(response.add_to_offer);
    }
});    
```

## Step 2: Extend storm.phalanax.actions.js

In case you are creating a hoplite in another bundle skip this step and jump directly to step 2a.

Make sure to register your new actions and create an instance of it.

``` 
/**
    Document        : storm.phalanx.actions
    Author          : mkr, dho
    Dependencies    : storm.phalanx.controller
    Requirements    : storm.phalanx.parser
    Description:
        Actions for the Phalanx system

    This file is like a bridge between hoplites and whole system. Hoplites
    communicates with the system to achieve goals together.
*/

// create an instance of phalanxParser
var phalanxInit = new phalanxParser;

/**
 * Object with types and hoplite name (which hoplite use for which TYPE).
 * As you can each hoplite can have more than one TYPE (action) for example
 * basket hoplite has: add_basket and get_basket.
 *
 * To make your hoplite work, please put it on this list.
 */
var phalanxSettings = {
    'get_price'                                     : 'price',
    'add_basket'                                    : 'basket',
    'get_basket'                                    : 'basket',
    'sort_basket'                                   : 'basket',
    'all_to_basket'                                 : 'basket',
    'add_stored_basket'                             : 'basket',
    'get_stored_baskets_list'                       : 'basket',
    'remove_stored_basket'                          : 'basket',
    'remove_basket_line_from_stored_basket'         : 'basket',
    'get_basket_variant'                            : 'basket',
    'load_list'                                     : 'list',
    'get_sidebox'                                   : 'sidebox',
    'validate_step'                                 : 'checkout',
    'get_delivery_from_invoice'                     : 'checkout',
    'get_existing_delivery'                         : 'checkout',
    'get_empty_delivery'                            : 'checkout',
    'get_chart_data'                                : 'dashboard',
    'get_overview_data'                             : 'dashboard',
    'get_tab_data'                                  : 'dashboard',
    'get_panel_data'                                : 'dashboard',
    'get_variants'                                  : 'variant',
    'get_line'                                      : 'quickorder',
    'get_search_result'                             : 'search',
    'add_to_offer'                                  : 'offer' // <--- our action
};

/**
 * You have to create an instance of your hoplite before you initialize
 * phalanxParser to be sure everything is working fine.
 */

// create an instance of price hoplite
phalanxInit.objHoplite['price'] = new hoplitePrice;

// create an instance of basket hoplite
phalanxInit.objHoplite['basket'] = new hopliteBasket;

// create an instance of list hoplite
phalanxInit.objHoplite['list'] = new hopliteList;

// create an instance of sidebox hoplite
phalanxInit.objHoplite['sidebox'] = new hopliteSidebox;

// create an instance of checkout hoplite
phalanxInit.objHoplite['checkout'] = new hopliteCheckout;

// create an instance of dashboard hoplite
phalanxInit.objHoplite['dashboard'] = new hopliteDashboard;

// create an instance of variants hoplite
phalanxInit.objHoplite['variants'] = new hopliteVariants;

// create an instance of quickorder hoplite
phalanxInit.objHoplite['quickorder'] = new hopliteQuickorder;

// create an instance of search hoplite
phalanxInit.objHoplite['search'] = new hopliteSearch;

// create an instance of offer hoplite
phalanxInit.objHoplite['offer'] = new hopliteOffer; // <-- our new hoplite

// initialize the phalanxParser (start the parser)
phalanxInit.init();
```

## Step 2a: Create (or extend) actions file in another / custom bundle

If storm.phalanax.actions.js doesn't exists in you custom bundle make sure to create it. 

``` 
/**
    Document        : storm.phalanx.actions
    Author          : mkr
    Dependencies    : storm.phalanx.controller
    Requirements    : storm.phalanx.parser
    Description:
        Actions for the Phalanx system
    This file is like a bridge between hoplites and whole system. Hoplites
    communicates with the system to achieve goals together.
*/
// create an instance of phalanxParser
var phalanxInit = new phalanxParser;
/**
 * Object with types and hoplite name (which hoplite use for which TYPE).
 * As you can each hoplite can have more than one TYPE (action) for example
 * basket hoplite has: add_basket and get_basket.
 *
 * To make your hoplite work, please put it on this list.
 */
var phalanxSettings = $.extend({}, phalanxSettings, {
  'add_to_offer' : 'offer'
});
/**
 * You have to create an instance of your hoplite before you initialize
 * phalanxParser to be sure everything is working fine.
 */
// create an instance of offer hoplite
phalanxInit.objHoplite['offer'] = new hopliteOffer;
// initialize the phalanxParser (start the parser)
phalanxInit.init();
```

## Step 2: Include new files in pagelayout

Two this to keep in mind in that step:

1.  Make sure the default hoplites and whole phalanx is also loaded
2.  Make sure your new hoplite(s) is loaded before any of the storm.phalanx.actions.js file

``` 
{% block javascripts %}
  {% javascripts
    ...
    'bundles/silversolutionseshop/js/phalanx/storm.phalanx.controller.js'
    'bundles/silversolutionseshop/js/phalanx/storm.phalanx.parser.js'
    'bundles/silversolutionseshop/js/phalanx/storm.phalanx.provider.js'
    'bundles/silversolutionseshop/js/phalanx/storm.phalanx.utils.js'
    'bundles/silversolutionseshop/js/phalanx/hoplite/price/price.js'
    'bundles/silversolutionseshop/js/phalanx/hoplite/basket/basket.js'
    'bundles/silversolutionseshop/js/phalanx/hoplite/list/list.js'
    'bundles/silversolutionseshop/js/phalanx/hoplite/sidebox/sidebox.js'
    'bundles/silversolutionseshop/js/phalanx/hoplite/checkout/checkout.js'
    'bundles/silversolutionseshop/js/phalanx/hoplite/dashboard/dashboard.js'
    'bundles/silversolutionseshop/js/phalanx/hoplite/variants/variants.js'
    'bundles/silversolutionseshop/js/phalanx/hoplite/quickorder/quickorder.js'
 
    'bundles/offerbundle/js/phalanx/hoplite/offer/offer.js' // <-- include hoplite
 
    'bundles/silversolutionseshop/js/phalanx/storm.phalanx.actions.js'

    'bundles/offerbundle/js/phalanx/storm.phalanx.actions.js'  // <-- include custom actions
    ...
  %}
    
  <script type="text/javascript" src="{{ asset_url }}"></script>
  {% endjavascripts %}
{% endblock %}
```

# PHP Part

![](attachments/23560533/23563807.png)

## Workflow

Phalanx

For a detailed usage of Phalanx, please see: <http://www.extranet.silversolutions.de:8090/display/EX/4.4+Phalanx>

For the detailed information about how to create a hoplite, see: <http://www.extranet.silversolutions.de:8090/display/EX/4.4.1+How+to+create+a+hoplite>

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
    
      - Every SubController must extend* Silversolutions\\Bundle\\EshopBundle\\Controller\\**BaseController*
    
      - Every SubController method excepts following paramaters: *Request $request, array $data = array()  
        *
    
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
      - Example: *Silversolutions\\Bundle\\ProjectBundle\\Controller\\AjaxProjectBasketController* extends *Silversolutions\\Bundle\\EshopBundle\\Controller\\AjaxBasketController*
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

A response of GET requests could be cached. To cache it you should [use HttpCachingServiceStrategy](https://doc.silver-eshop.de/display/EZC14/HTTP+caching) in your Ajax**\<Type\>**Controller.

AjaxController copies **all http headers** from Ajax**\<Type\>**Controller subresponse to the final response. It is done to pass caching headers to reverse proxy.
