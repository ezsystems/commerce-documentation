# Price engine cookbook

## How to use the price engine

The entry point to the price engine is the [ChainPriceService](../price_engine_api/price_engine_services/chainpriceservice/chainpriceservice.md). If you want to get prices from the price engine, you have to use this service. See this [Recipe](recipe_how_to_work_with_pricerequest_and_priceresponse.md) to find out, how to work with PriceReuest and PriceResponse.

Example:

``` php
public function getPricesAction()
{
    //create new PriceRequest instance with appropriate data
    $priceRequest = $this->createPriceRequest(array($catalogElement), array(1));
    $contextId = 'product_detail';

    try {
        $priceService = $this->get('siso_price.price_service.chain');
        /** @var PriceResponse $priceResponse **/
        $priceResponse = $priceService->getPrices($priceRequest, $contextId);
    } catch(Siso\Bundle\PriceBundle\Exception\PriceRequestFailedException $e) {
        //...no prices were provided by any price provider
    }

    //@todo - evaluate the $priceResponse, e.g. assign the prices to your catalog element
    
}
```

## How to get prices for catalog elements?

If you want to use the price engine to get the prices and stock information for your catalog element(s), the most easy way is to use the `CatalogService`, that already contains helper methods and will create the PriceRequest for you.

It will also already assign the correct prices and stock back to your catalog element(s), so you don't have to take care about it.

``` php
public function getPricesForCatalogElement($catalogElement)
{
    /** @var CatalogService $catalogService */
    $catalogService = $this->container->get('silver_catalog.catalog_service');
    /** @var CustomerProfileDataServiceInterface $customerProfileDataService */
    $customerProfileDataService = $this->get('ses.customer_profile_data.ez_erp');

    $productNodeArray = array($catalogElement);
    $quantity= 1;
    $contextId = 'product_detail';
    $customerProfileData = $customerProfileDataService->getCustomerProfileData();
    $customerCurrency = $customerProfileData->sesUser->customerCurrency;

    $catalogService->addPricesToProductNodes($productNodeArray, $customerCurrency, $contextId,
        array($quantity));

    //catalog element has correct prices and stock
    return $catalogElement;
}
```

## How to check which price provider has been used in the basket

Often it is useful to know which price provider was used to calculate a price (e.g. int he basket). A use case is e.g. to enable special payment methods if it can be ensured that the ERP was able to provide realtime prices or even to disable the checkout in case the ERP is not able to provide prices. 

The basket contains an attribute "priceResponseSourceType" which contains the source type of the price provider:

![](../../img/price_engine_2.png)

"remote" means that the ERP system has provided the prices for the current basket. 

## How to write a new Price Provider

see [Price Providers](../price_engine_api/price_engine_services/chainpriceservice/price_providers/price_providers.md).

### Step 1

Implement new Price Provider class. It needs to implement `PriceProviderInterface`. Extend `AbstractPriceProvider` if you want to use some common code for calculating totals. See this [Recipe](recipe_how_to_implement_new_price_provider.md) to find out, what you exactly need to implement.

```
class CustomPriceProvider extends AbstractPriceProvider implements PriceProviderInterface
```

### Step 2

Add new service definition and tag it with `siso_price.price_provider`.

`vendor/silversolutions/silver.e-shop/src/Siso/Bundle/PriceBundle/Resources/config/services.xml`:

``` xml
<parameter key="siso_price.price_provider.custom.class">Siso\Bundle\PriceBundle\Service\CustomPriceProvider</parameter>
 
<service id="siso_price.price_provider.custom" class="%siso_price.price_provider.custom.class%">
    <argument type="service" id="..." />
    <tag name="siso_price.price_provider" />
</service>
```

### Step 3

Inject your service into the chain of price providers

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/silver.eshop.yml`:

``` yaml
siso_price.default.price_service_chain.basket:
    - siso_price.price_provider.custom
    - siso_price.price_provider.remote
    - siso_price.price_provider.local
```

## How to configure chain of Price Providers

In order to use different providers depending on the context (e.g. basket page, product detail page) we need to set up configuration:

`vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/silver.eshop.yml`:

``` yaml
siso_price.default.price_service_chain.product_list:
    - siso_price.price_provider.local

siso_price.default.price_service_chain.product_detail:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local

siso_price.default.price_service_chain.slider_product_list:
    - siso_price.price_provider.local

siso_price.default.price_service_chain.basket:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local

siso_price.default.price_service_chain.stored_basket:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local

siso_price.default.price_service_chain.wish_list:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local

siso_price.default.price_service_chain.quick_order:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local

siso_price.default.price_service_chain.comparison:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local
```

In the example above if the context, in which we want to calculate prices, is product detail page, then the price engine will call remote price provider to get proper prices first.

If remote price provider fails to deliver then local price provider will calculate those prices.

Other scenarios could be to added, like a cache price provider, which could first check if prices are available in a cache.

#### Configure the service id of the remote price provider

Do not forget to configure the service id of the remote price provider. This configuration will be used to set error message in the basket, if the remote price provider was not able to calculate the prices.

``` yaml
parameters:
    #configure the service id of the remote price provider
    siso_price.default.price_service_chain_remote: siso_price.price_provider.remote
```

## How to pass additional information to priceRequest from catalogElement

In order to enhance the `priceRequest` with additional information, which in projects could be required to correctly calculate the price, we use an event listener concept.

The listener `PriceLineRequestListener` that works with `PriceLineRequestEvent` will add additional information from catalog element, for each attribute specified in configuration:

``` yaml
#list of catalog element attributes, that should be additionally stored in the extended data of the request price line
siso_price.default.price_request_extended_data: [ 'path', 'otherAttribute']
```

The listener, for every `PriceLine` object inside `PriceRequest`, searches both in `catalogElement` attributes and `dataMap` for those attributes. If they are available, it will add them into `ExtendedData` of every `PriceLine` object.

There is also a possibility to add information to `PriceRequest` itself, not only for every `PriceLine` object. For this you should implement event listener for `PriceRequestEvent`.

For reference how to write listener see implementation for `PriceLineRequestListener`.

## Price calculation events

``` php
/**
 * Defines constants for price events.
 */
final class PriceEvents
{
    /**
     * This event must be dispatched before price line request is sent
     *
     * Events in the standard implementation:
     * - After a price request line was completely constructed in the basket
     *   price calculation (in the BasketService)
     * - After a price request line was completely constructed in the catalog
     *   price calculation (in the CatalogService)
     *
     * @see Siso\Bundle\PriceBundle\Event\PriceLineRequestEvent
     * @var string
     */
    const PRICE_LINE_REQUEST = 'siso_price.price_line_request';

    /**
     * This event must be dispatched before price request is sent
     *
     * Events in the standard implementation:
     * - After a price request was completely constructed in the basket
     *   price calculation (in the BasketService)
     * - After a price request was completely constructed in the catalog
     *   price calculation (in the CatalogService)
     *
     * @see Siso\Bundle\PriceBundle\Event\PriceRequestEvent
     * @var string
     */
    const PRICE_REQUEST = 'siso_price.price_request';
}
```

Example listener service definition"

``` xml
<service id="custom.price_line_request_listener" class="%custom.price_line_request_listener.class%">
    <tag name="kernel.event_listener" event="siso_price.price_line_request" method="onPriceLineRequest" />
</service>
```

Example listener implementation:

``` php
namespace Example\EventListener;

use Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement;
use Siso\Bundle\PriceBundle\Event\PriceLineRequestEvent;

/**
 * Example listener
 */
class PriceLineRequestListener
{

    /**
     * @param PriceLineRequestEvent $event
     */
    public function onPriceLineRequest(PriceLineRequestEvent $event)
    {
        $catalogElement = $event->getCatalogElement();
        $priceLine = $event->getPriceLine();

        $priceLine->setExtendedData(
            array(
                'new_field' => $catalogElement->getField('new_field')->toString(),
            )
        );
    }
}
```
 
