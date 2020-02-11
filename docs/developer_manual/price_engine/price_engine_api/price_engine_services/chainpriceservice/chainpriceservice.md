#  ChainPriceService 

## Goal

This service is the base entry point to the price engine. Via this service you get prices depending on your $contextId. The $contextId is a parameter that indicates the context in which the prices should be calculated, e.g. basket, product list or product detail.

The ChainPriceService does not calculate the prices by itself. It calls [price providers](Price-Providers_23560237.html) in a chain depending on the [configuration](#ChainPriceService-chain_config).

If an error occurs the price provider throws an exception and then next provider in the chain will be used.

Immediately if the first provider returns prices, they are returned back to the 'caller'. The next price provider is not executed.

If **none** of the price providers is able to return prices, an exception is thrown and **empty prices** and **empty stock** must be set.

## Configuration

**Service ID**

``` 
siso_price.price_service.chain 
```

<span id="ChainPriceService-chain_config" class="confluence-anchor-link">

**Chain configuration example for basket**

``` 
#the last part of the parameter name (here basket) must match the $contextId!
siso_price.default.price_service_chain.basket:
    - siso_price.price_provider.remote
    - siso_price.price_provider.local 
```

# API: PriceServiceInterface

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>public function getPrices(PriceRequest $priceRequest, $contextId);</code></pre></td>
<td><pre><code>This method will always return an instance of PriceResponse or
throw the PriceRequestFailedException.

The price service will determine a price provider, depending on the
given context, that will perform the price calculations. If no
provider was able to properly calculate the prices, it will throw a
PriceRequestFailedException.</code></pre></td>
</tr>
</tbody>
</table>
