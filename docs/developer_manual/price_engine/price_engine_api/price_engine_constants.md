# Price engine constants

## Introduction

Constants are used throughout the shop for consistent naming for price related request and response values.

All the constants for price engine are stored in final class:

``` 
\Silversolutions\Bundle\EshopBundle\Model\Price\PriceConstants
```

## Types of constants

|Type|Description|
|--- |--- |
|`PRICE_ENGINE_SOURCE_*`|source of price calculation e.g. ERP, Shop, Request|
|`PRICE_REQUEST_PRICE_TYPE_*`|type of price e.g. list, custom, base|
|`PRICE_RESPONSE_LINE_TYPE_*`|type of price line e.g. product, shipping|
|`PRICE_RESPONSE_TOTALS_*`|type of total value e.g. sum, lines, additional_lines|
|`PRICE_RESPONSE_SOURCE_TYPE_*`|indicates which price provider calculated the prices|
