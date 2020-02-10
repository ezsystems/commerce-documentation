#  StandardPriceFactory 

## Introduction

Depending on the configuration, it will create Price instance with price inclusive VAT or exclusive VAT. The configuration can be overridden by method parameters.

The default fallback price source depends on the content of the price fields.

  - If one of them is empty (zero or null), it's: PriceConstants::PRICE\_ENGINE\_SOURCE\_INCOMPLETE
  - Else it's: PriceConstants::PRICE\_ENGINE\_SOURCE\_LOCAL

## Creating Price

There are 2 possibilities to create Price:

1.  create Price from PriceLine in the PriceRequest. It uses the content of a PriceLine and the given currency code. 

    ``` 
    /**
     * @param PriceLine $responseLine
     * @param string $currencyCode  Must be set with an ISO 4217 value
     * @param string $type  Must be one of the PRICE_RESPONSE_PRICE_TYPE_* values of
     *                      PriceConstant class. It defines, which of the
     *                      prices in the response line is referred to.
     * @param bool $useUnitPrice If true unit price will be used instead of line amount
     * @return Price
     */
    public function createPriceFromResponseLine(PriceLine $responseLine, $currencyCode, $type, $useUnitPrice = false);
    ```

2.  create Price from given properties. The necessary values are passed directly within an array. 
    
    Logic for '**vatPercent**' calculation  
    \- if this value is set, it will be used  
    \- if it is not set, but 'vatCode' is set, 'vatCode' and 'country' will be used to get the vatPercent  
    \- otherwise fallback vatPercent (determined by fallback country and fallback vat code) is used  

    ``` 
    /**
     * @param array $properties
     * @return Price
     */
    public function createPrice(array $properties);
    ```

    ``` 
    // Example of all possible properties
    $properties = array(
          'price' => 1.0,
          'priceInclVat' => 1.0
          'priceExclVat' => 0.81,
          'isVatPrice' => true,
          'vatPercent' => 19,
          'vatCode' => 'vegetable',
          'country' => 'DE',
          'currency' => 'EUR',
          'source' => 'Shop'
     );
    ```

## Default Configuration

Factory uses default values for fallbacks if value is missing.

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/silver.eshop.yml**

``` 
siso_core.default.standard_price_factory.fallback_currency: EUR
siso_core.default.standard_price_factory.fallback_country: DE
siso_core.default.standard_price_factory.fallback_vat_code: vegetable
siso_core.default.standard_price_factory.is_vat_price: true
```
