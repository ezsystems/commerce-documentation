#  VatServiceInterface 

## Introduction

The purpose of VAT Service is to do VAT calculations and return VAT percent as float value.

## API: VatServiceInterface

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
<td><pre><code>public function getVatPercentForPriceRequest($lineId, PriceRequest $priceRequest);</code></pre></td>
<td><pre><code>Returns the VAT percent(%) value for priceRequest according to a specific logic.</code></pre></td>
</tr>
<tr>
<td><pre><code>public function getVatPercent($country, $vatCode);</code></pre></td>
<td><pre><code>Returns the VAT percent(%) value for a given $country and $vatCode.</code></pre></td>
</tr>
</tbody>
</table>
