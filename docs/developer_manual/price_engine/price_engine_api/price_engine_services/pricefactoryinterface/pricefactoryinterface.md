# PriceFactoryInterface

## Introduction

The purpose of this service is to create Price instances in the eZ Commerce.

!!! caution

    Do not create Price Field directly. Use Price Factory instead.

## API: PriceFactoryInterface

|Method|Description|
|--- |--- |
|public function createPriceFromResponseLine(PriceLine $responseLine, $currencyCode, $type, $useUnitPrice = false);|This method instantiates a Price entity.</br>It uses the content of a PriceLine and the given currency code.|
|public function createPrice(array $properties);|This method instantiates a Price entity.</br>The necessary values are passed directly within an array.|
