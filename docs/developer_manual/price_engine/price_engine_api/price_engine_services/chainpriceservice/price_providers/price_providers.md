#  Price Providers 

## Goal

A price provider will fetch or calculate the prices for the price request. If an error occurred or the prices could not be calculated properly, it will throw a PriceCalculationFailedException, which gives responsibility back to the [price service](ChainPriceService_23560686.html).

When providing the prices, every price provider must return both: 

  - list\_price
  - custom\_price

Tag: PriceProvider

All price providers must use following tag:

    siso_price.price_provider

## API: PriceProviderInterface

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
<td><pre><code>public function calculatePrices(PriceRequest $priceRequest);</code></pre></td>
<td><pre><code>This method will always return an instance of PriceResponse or
throw the PriceCalculationFailedException.

A price provider will fetch or calculate the prices for the price
request. If an error occurred or the prices could not be calculated
properly, it will throw a PriceCalculationFailedException, which
gives responsibility back to the price service.</code></pre></td>
</tr>
</tbody>
</table>
