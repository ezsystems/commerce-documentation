#  PriceFactoryInterface 

## Introduction

The purpose of this service is to create Price instances in the eZ Commerce.

**Do not create Price Field directly \! Use Price Factory instead \!\!**

## API: PriceFactoryInterface

<div class="line number1 index0 alt2">

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
<td><pre><code>public function createPriceFromResponseLine(PriceLine $responseLine, $currencyCode, $type, $useUnitPrice = false);</code></pre></td>
<td><pre><code>This method instantiates a Price entity.
It uses the content of a PriceLine and the given currency code.</code></pre></td>
</tr>
<tr>
<td><pre><code>public function createPrice(array $properties);</code></pre></td>
<td><pre><code>This method instantiates a Price entity.
The necessary values are passed directly within an array.</code></pre></td>
</tr>
</tbody>
</table>
