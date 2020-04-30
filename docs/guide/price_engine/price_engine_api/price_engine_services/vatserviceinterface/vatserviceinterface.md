# VatServiceInterface

## Introduction

The purpose of VAT Service is to do VAT calculations and return VAT percent as float value.

## API: VatServiceInterface

|Method|Description|
|--- |--- |
|public function getVatPercentForPriceRequest($lineId, PriceRequest $priceRequest);|Returns the VAT percent(%) value for priceRequest according to a specific logic.|
|public function getVatPercent($country, $vatCode);|Returns the VAT percent(%) value for a given $country and $vatCode.|
