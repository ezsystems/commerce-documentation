#  Price Engine Constants 

## Introduction

Constants are used throughout the shop for consistent naming for price related request and response values.

All the constants for price engine are stored in final class:

``` 
\Silversolutions\Bundle\EshopBundle\Model\Price\PriceConstants
```

## Types of constants

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>PRICE_ENGINE_SOURCE_*</code></pre></td>
<td>source of price calculation e.g. ERP, Shop, Request</td>
</tr>
<tr>
<td><pre><code>PRICE_REQUEST_PRICE_TYPE_*</code></pre></td>
<td>type of price e.g. list, custom, base</td>
</tr>
<tr>
<td><pre><code>PRICE_RESPONSE_LINE_TYPE_*</code></pre></td>
<td>type of price line e.g. product, shipping</td>
</tr>
<tr>
<td><pre><code>PRICE_RESPONSE_TOTALS_*</code></pre></td>
<td>type of total value e.g. sum, lines, additional_lines</td>
</tr>
<tr>
<td><pre><code>PRICE_RESPONSE_SOURCE_TYPE_*</code></pre></td>
<td>indicates which price provider calculated the prices</td>
</tr>
</tbody>
</table>
