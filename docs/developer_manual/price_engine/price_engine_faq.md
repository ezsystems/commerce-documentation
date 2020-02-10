#  Price Engine - FAQ 

Does the price engine support volume based prices?

It depends on the Priceprovider.

The Priceprovider ERP will provide volume based prices as defined in the ERP.

How do i get delivery costs or additional costs?

The price provider returns such a informations as *additional lines* with a special *type*.

``` 
$additionalLines = array();
$additionalLines[] = $this->createShippingPriceLine();
$priceResponse?setAdditionalLines($additionalLines);
/**
     * creates a PriceLine for shipping costs
     *
     * @return PriceLine
     */
    protected function createShippingPriceLine()
    {
        $priceLine = new PriceLine();
        $priceLine->setType(PriceConstants::PRICE_RESPONSE_LINE_TYPE_SHIPPING);
        $shippingCost = $this->shippingCostCalculator->calculateShipping();
        $shippingVatCountry = $this->configResolver->getParameter('shipping_vat_country', 'siso_core');
        $shippingVatCode = $this->configResolver->getParameter('shipping_vat_code', 'siso_core');
        $vatPercent = $this->vatService->getVatPercent($shippingVatCountry, $shippingVatCode);

        //create PriceLineAmounts
        $shippingPrice = new PriceLineAmounts();
        $shippingCostVat = ($shippingCost * $vatPercent) / 100;
        $shippingCostNet = $shippingCost - $shippingCostVat;
        $shippingPrice->setLineAmountGross($shippingCost);
        $shippingPrice->setLineAmountNet($shippingCostNet);
        $shippingPrice->setLineAmountVat($shippingCostVat);
        $shippingPrice->setUnitPriceGross($shippingCost);
        $shippingPrice->setUnitPriceNet($shippingCostNet);
        $shippingPrice->setUnitPriceVat($shippingCostVat);
        // ToDo: Should we create a new price constant?
        $shippingPrice->setSource(PriceConstants::PRICE_ENGINE_SOURCE_LOCAL);
        $prices[PriceConstants::PRICE_RESPONSE_PRICE_TYPE_CUSTOM] = $shippingPrice;
        $prices[PriceConstants::PRICE_RESPONSE_PRICE_TYPE_LIST] = $shippingPrice;
        $priceLine->setPrices($prices);
        // ToDO: check if we need to get the VAT for shipping costs.
        $priceLine->setVatPercent($vatPercent);
        $extendedData = array(
            'LineType' => 1,
            'CostType' => PriceConstants::PRICE_RESPONSE_LINE_TYPE_SHIPPING,
            'StockNumeric' => '',
            'AvailabilityColor' => '',
            'VatPercent' => $vatPercent,
            'PriceAmountGross' => $shippingCost,
            'PriceIsIncVat' => 1,
            'BelongsToLine' => '',
            'name' => 'msg.shipping_cost_name'
        );
        $priceLine->setExtendedData($extendedData);

        return $priceLine;
    }
```

![](attachments/23560383/23563288.png)

How can i access stock information?

You can get stock information from the PriceLine in the PriceResponse

``` 
$priceResponse = $priceService->getPrices($priceRequest, $contextId);

foreach ($priceResponse->getLines() as $priceLine) {
    //get stock
    $stockNumeric = $priceLine->getStockNumeric();
    if (isset($stockNumeric)) {
        $stock = new StockField(array('stockNumeric' => $stockNumeric));
        $productNode->setStock($stock);
    }
}
```

How the currency is handled?

It depends on the Priceprovider.

  - LocalPriceProvider is using the customer currency that is set in the PriceRequest.
  - RemotePriceProvider is using the currency returned from the ERP and if not set, it is also using the customer currency.

How can i pass additional information to the price provider?

If your price provider needs some additional information, you can provide them in several ways:

  - On the top level in *extendedData*

    ``` 
    $priceRequest = new PriceRequest();
    $extendedData = array(
        'customerId' => 126,
        'email' => 'test@testaccount.com'
    );
    
    $priceRequest->setExtendedData($extendedData);
    ```

     

  - On the line level in  *extendedData* 

    ``` 
    $priceLine = new PriceLine();
    $extendedData = array(
       'remark'  => $customerRemark
    );
    
    $priceLine->setExtendedData($extendedData);
    ```

     

  - You can also pass additional data in the parties, if they are connected to a customer

    ``` 
    $priceRequest = new PriceRequest();
    $buyerParty = $customerProfileDataService->getDefaultBuyerParty();
    $buyerParty->SesExtension->value['customerGroup'] = 'WIKI';
    
    $priceRequest->setBuyerParty($buyerParty);
    ```

How can i find out which provider did calculate my prices?

If the price provider calculated the prices, it will set a source type in the PriceResponse. This source type can be evaluated.

Possible source types:

    PriceConstants::PRICE_RESPONSE_SOURCE_TYPE_REMOTE = 'remote'

    PriceConstants::PRICE_RESPONSE_SOURCE_TYPE_LOCAL = 'local'

``` 
//if the prices were not calculated by a remote source, set an error message in the basket
if ($priceResponse->getSourceType() !== PriceConstants::PRICE_RESPONSE_SOURCE_TYPE_REMOTE) {
    $basket->setErrorMessage(
        $this->transService->translate('Remote prices can not be calculated!')
    );
}
```

What happens in case the ERP is not available?

For the basket the LocalPriceProvider will calculate prices as a fallback. The customer will see an error message that the realtime prices are not available at the moment - if the remote price provider was configured in the chain on the first place. See: [When an error message is shown?](#PriceEngine-FAQ-error_message)

When an error message is shown?

When the chain for your context id was configured in a way, that the first provider is remote and the remote price calculation fails, en error message might be displayed. The shop will check the chain for your context id. So it is possible that in the basket there is an error message displayed if the remote price calculation failed, but not in the wishlit.

You have to configure the service id of the remote price provider\!

``` 
#configure the service id of the remote price provider
siso_price.default.price_service_chain_remote: siso_price.price_provider.remote

siso_price.default.price_service_chain.basket:
 - siso_price.price_provider.remote
    - siso_price.price_provider.local

siso_price.default.price_service_chain.stored_basket:
 - siso_price.price_provider.local

siso_price.default.price_service_chain.wish_list:
 - siso_price.price_provider.local
```

Why can user see/not see prices on product detail page with/without the necessary role?

Caching has to be configured to use user-hash in order to display the correct product detail page to anonymous or registered users. If this is not done, the first call will be cached for all users\!

Why can user not see prices in sliders, catalog list, product detail or comparison?

They don't have the necessary role to see prices, it has to be configured for users in the backend. (siso\_policy/see\_product\_price)

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-06-11 um 10.57.16.png](attachments/23560383/23563489.png) (image/png)  
![](images/icons/bullet_blue.gif) [add.png](attachments/23560383/23563288.png) (image/png)  
