# Price engine

## Introduction

The Price Engine is responsible for calculating all kinds of prices in the shop. It is able e.g. to calculate prices based on imported prices and rules, but also to use the business logic of the ERP system which is connected to the eZ Commerce. 

It offers a very flexible way to combine the logic of an ERP system and a local price provider in order to get the best compromise between realtime data and a performant shop. In addition it offers a failover concept in case of the ERP is not available. 

!!! tip

    The base entry point for price engine is [ChainPriceService](price_engine_api/price_engine_services/chainpriceservice/chainpriceservice.md), which is used to fetch prices.

It determines a chain of Price Providers, which will be responsible for calculating the prices. 

It is up to configuration, which set of Price Providers will be used. This is important because depending on the page (product listing, product detail, checkout etc.)  different requirements have to be solved:

- on a **product list** a lot of prices might have to be calculated. This might cause problems when using the Business logic of the ERP. In this case a local Price Provider will be fast in order to provide e.g. list prices
- on a **product detail** page the customer expects to get his individual price from the ERP
- in the **basket** the price shall always be provided by the ERP

The ChainPriceService is using *[ContextId](../../glossary/term_contextid.md)* (e.g. basket, product\_list). For each context a prioritized  list of Price Providers can be defined. This concept also allows to define a fallback if e.g. the ERP is not available. The response for a price request contains a source so that the shop can display e.g. a warning if the price and stock is not provided by the ERP but by a fallback price provider. 

In addition to prices, the ChainPriceService is able to retrieve stock information since the ERP systems usually provide this information in the price request. 

## Available price providers

|Provider|Logic|Note|
|--- |--- |--- |
|Local price provider|A simple one getting the price from the product itself|Do not use this provider for eZ Commerce. Please use ShopPricdEngine instead|
|Shop price provider|A more sophisticated price provider. Offers currency and customer group support||
|Remote price engine|Gets prices from the ERP|Advanced version only|

## How to get prices from the price engine

This example shows how to get a price from the ERP for a given SKU "D4142". 

- First the productNode is fetched by using fetchElementBySku()
- Afterwards the price engine is called using the priceContext "basket". 
- The method addPricesToProductNodes() will enrich the ProductNode with customer specific prices and stock infos

For basket a realtime request to the ERP is configured:

``` yaml
siso_price.default.price_service_chain.basket:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local
```

``` php
$skuList = array('D4142');
$catalogService = $this->getContainer()->get('silver_catalog.data_provider_service');
foreach ($skuList as $sku) {
    /** @var $catalogElement \Silversolutions\Bundle\EshopBundle\Product\ProductNode */
    $catalogElement = $catalogService->getDataProvider()->fetchElementBySku(
        $sku,
        array()
    );

    if ($catalogElement instanceof \Silversolutions\Bundle\EshopBundle\Catalog\CatalogElement) {
        $productNodeArray[] = $catalogElement;
    }
}

$catalogService = $this->getContainer()->get('silver_catalog.catalog_service');
$catalogService->addPricesToProductNodes($productNodeArray, "EUR", "basket",
    array(1));
$price = $productNodeArray[0]->customerPrice->price->price;
$isOnStock =$productNodeArray[0]->stock->isOnStock();
echo "Price: $price";
echo "OnStock: ".$isOnStock;
```

If you do not want to fetch a sku using the dataprovider you have to create a ProductNode manually:

``` php
$price = new \Siso\Bundle\PriceBundle\Model\Price(
    array(
        'price' => 12.00,   // Fallback price
        'isVatPrice' => true,
        'vatPercent' => (float)19,
        'currency' => 'EUR',
        'source' => 'ERP',
    )
);
$priceField = new \Silversolutions\Bundle\EshopBundle\Content\Fields\PriceField(array('price' => $price));
$attributes = array(
    'sku'     => 'D4142',
    'vatCode' => 'VATNORMAL',
    'price'   => $priceField
);
$catalogElement = new \Silversolutions\Bundle\EshopBundle\Product\OrderableProductNode($attributes, $urlService);
## If a variant is used
$variantCode = "001";
$variantService = $this->get('silver_catalog.variant_service');
$catalogElement = $variantService->createOrderableProductFromVariant($catalogElement, $variantCode);
```

## Before you start 

Please keep in mind that the Price Service is really connected with a lot of different modules in our shop. Be sure to check these out:

- [CustomerProfileData](../customers/customers.md)
