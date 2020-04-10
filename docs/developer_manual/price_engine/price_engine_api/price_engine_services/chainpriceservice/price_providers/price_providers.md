# Price providers

## Goal

A price provider will fetch or calculate the prices for the price request. If an error occurred or the prices could not be calculated properly, it will throw a PriceCalculationFailedException, which gives responsibility back to the [price service](../chainpriceservice.md).

When providing the prices, every price provider must return both: 

- `list_price`
- `custom_price`

!!! note "Tag: PriceProvider"

    All price providers must use following tag:

    `siso_price.price_provider`

## API: PriceProviderInterface

|Method|Description|
|--- |--- |
|public function calculatePrices(PriceRequest $priceRequest);|This method will always return an instance of PriceResponse or throw the PriceCalculationFailedException.</br>A price provider will fetch or calculate the prices for the price request. If an error occurred or the prices could not be calculated properly, it will throw a PriceCalculationFailedException, which gives responsibility back to the price service.|
