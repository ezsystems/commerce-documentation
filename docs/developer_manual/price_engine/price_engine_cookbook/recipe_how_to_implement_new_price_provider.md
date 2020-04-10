# How to implement new price provider

See [Cookbook](price_engine_cookbook.md) to find out how to write a new Price Provider.

## What to implement?

You will need to implement the method *calculatePrices*. The goal of this method is to provide *PriceResponse* object, the way how you get the data and calculate the prices is up to you. You can inject other services, that will do some job for you, or just use the data provided in the *PriceRequest*.

In our example we just use the provided data and make usage of the [VatService](VatServiceInterface_23560246.html) to get the vatPercent.

``` php
/**
 * {@inheritdoc}
 *
 * @param PriceRequest $priceRequest
 * @throws PriceCalculationFailedException
 * @return PriceResponse
 */
public function calculatePrices(PriceRequest $priceRequest)
{
    $priceResponseLines = array();
    $additionalLines = array();

    $priceLines = $priceRequest->getLines();
    /** @var PriceLine $priceLine */
    foreach ($priceLines as $priceLine) {
        //use vat service to get the vatPercent, may return something like 19.0
        $vatPercent = $this->vatService->getVatPercentForPriceRequest($priceLine->getId(), $priceRequest);
        $vatCode = $priceLine->getVatCode();
 
        $priceResponseLines[] = $this->createResponsePriceLine(
            $priceLine,
            $vatCode,
            $vatPercent,
            $priceRequest->getCustomerVatRequired()           
        );
    }

    $responseTotals = array();
    //if you extends the AbstractPriceProvider, you can use some build-in functions 
    $totalsLines = $this->calculateTotalsForLines($priceResponseLines);
    $totalsAdditional = $this->calculateTotalsForLines($additionalLines);
    $totalsSum = $this->calculateTotalsSum(array($totalsLines, $totalsAdditional));

    $responseTotals[PriceConstants::PRICE_RESPONSE_TOTALS_LINES] = $totalsLines;
    $responseTotals[PriceConstants::PRICE_RESPONSE_TOTALS_ADDITIONAL_LINES] = $totalsAdditional;
    $responseTotals[PriceConstants::PRICE_RESPONSE_TOTALS_SUM] = $totalsSum;

    $priceResponse = new PriceResponse();
    $priceResponse->setGeneralCurrencyCode($priceRequest->getCustomerCurrency());
    $priceResponse->setSourceType(PriceConstants::PRICE_RESPONSE_SOURCE_TYPE_LOCAL);
    $priceResponse->setTotals($responseTotals);
    $priceResponse->setAdditionalLines($additionalLines);
    $priceResponse->setLines($priceResponseLines);

    return $priceResponse;
}

private function createResponsePriceLine(
    PriceLine $priceLine,
    $vatCode,
    $vatPercent,
    $customerVatRequired
) {
    $priceLineAmounts = $priceLine->getPrices();

    //get the imported base price
    /** @var PriceLineAmounts $basePrice */
    $basePrice = $priceLineAmounts[PriceConstants::PRICE_REQUEST_PRICE_TYPE_BASE];

    //calculate the prices - this is up to you...
    $unitPriceGross = $basePrice->getUnitPriceGross;
    $unitPriceNet = $basePrice->getUnitPriceNet;
    $unitPriceVat = $unitPriceGross - $unitPriceNet;
    $lineAmountGross = $unitPriceGross * $priceLine->getQuantity();
    $lineAmountNet = $unitPriceNet * $priceLine->getQuantity();
    $lineAmountVat = $lineAmountGross - $lineAmountNet;

    //set the prices
    $price = new PriceLineAmounts();
    $price->setLineAmountGross((float) $lineAmountGross);
    $price->setLineAmountNet((float) $lineAmountNet);
    $price->setLineAmountVat((float) $lineAmountVat);
    $price->setUnitPriceGross((float) $unitPriceGross);
    $price->setUnitPriceNet((float) $unitPriceNet);
    $price->setUnitPriceVat((float) $unitPriceVat);

    $listPrice = price;
    $customerPrice = price;

    //each provider must provider both - list and customer price!
    $priceResponseLinePrices[PriceConstants::PRICE_RESPONSE_PRICE_TYPE_CUSTOM] = $customerPrice;
    $priceResponseLinePrices[PriceConstants::PRICE_RESPONSE_PRICE_TYPE_LIST] = $listPrice;

    $priceResponseLine = new PriceLine();
    $priceResponseLine->setType(PriceConstants::PRICE_RESPONSE_LINE_TYPE_PRODUCT);
    $priceResponseLine->setId($priceLine->getId());
    $priceResponseLine->setQuantity($priceLine->getQuantity());
    $priceResponseLine->setSku($priceLine->getSku());
    $priceResponseLine->setVariantCode($priceLine->getVariantCode());
    $priceResponseLine->setStockNumeric($priceLine->getStockNumeric());
    $priceResponseLine->setVatCode($vatCode);
    if (!$customerVatRequired) {
        $vatPercent = 0.0;
    }
    $priceResponseLine->setVatPercent($vatPercent);
    $priceResponseLine->setPrices($priceResponseLinePrices);
    $priceResponseLine->setExtendedData($priceLine->getExtendedData());

    return $priceResponseLine;
}
```
