#  ScaledPriceServiceInterface 

## Introduction

The purpose of Scaled Price Service is to calculate correct price, if there is a scaled price available. This is determined by attributes:

  - customerGroup (optional)
  - minQuantity \*
  - startDate (optional)
  - endDate (optional)
  - price \*
  - isInclVat \* 
  - priceGross (optional)
  - priceNet (optional)

## API: ScaledPriceServiceInterface

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
<td><pre><code>public function calculateScaledPrice($lineID, PriceRequest $priceRequest);</code></pre></td>
<td><pre><code>This method will always return matching scaled price PriceLineAmounts or 
return default from priceRequest</code></pre></td>
</tr>
</tbody>
</table>

## Examples in projects

##### Scaled prices for quantity only

![](plugins/servlet/confluence/placeholder/unknown-attachment "Bildschirmfoto 2015-05-13 um 10.08.12.png")

##### Scaled prices for user groups

![](plugins/servlet/confluence/placeholder/unknown-attachment "Bildschirmfoto 2015-05-13 um 10.42.03.png")

##### Scaled prices for user groups and quantity

![](plugins/servlet/confluence/placeholder/unknown-attachment "Bildschirmfoto 2015-05-13 um 10.45.49.png")
