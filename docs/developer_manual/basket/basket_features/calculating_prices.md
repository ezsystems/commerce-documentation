#  Calculating prices 

The basket recalculates the prices after each change of the basket. The price engine will be requested when the basket is stored. This avoids that the price engine is triggered several times (e.g. if more than one product has been updated or added). The prices provided by the price engine (and ERP) will be stored in the basket for each line.

By default the attributes from the ProductNode entity will be used. The price engine will set the following fields:

<table>
<thead>
<tr class="header">
<th><p>Field</p></th>
<th><p>Value</p></th>
<th><br />
 </th>
</tr>
</thead>
<tbody>
<tr>
<td>LinePriceAmount</td>
<td>The customer price for the given quantity</td>
<td><br />
</td>
</tr>
<tr>
<td>IsIncVat</td>
<td>If LinePriceAmount is inc VAT or not</td>
<td><br />
</td>
</tr>
<tr>
<td>Price</td>
<td>Price for one piece. This price may be calculated if the ERP doesn´t provide a price for one piece. This is the normal case since due to complex price rules an ERP might not be able to give a price for a single product.</td>
<td><br />
</td>
</tr>
<tr>
<td>Vat</td>
<td>VAT in %</td>
<td><br />
</td>
</tr>
<tr>
<td>Currency</td>
<td>The currency provided by the ERP/price engine</td>
<td><br />
</td>
</tr>
</tbody>
</table>

The price engine will set stock information as well since the ERP often provides information about the availability in a price request as well:

  - remoteDataMap\['stockNumeric'\] - number of items in stock
  - remoteDataMap\['onStock'\] - boolean

### Price recalculation

The price engine will also be triggered, when the basket is fetched from the database and the last price calculation is older than given minutes.  In addition the prices will be calculated each time the basket is changed.

The time is set in the configuration in minutes:

**\~EShopBundle/Resources/config/basket.yml**

<table>
<tbody>
<tr>
<td><p><code class="sourceCode php">parameters:</code><br />
<code class="sourceCode php">    </code><code class="sourceCode php">ses_basket.</code><code class="sourceCode php"><span class="kw">default</code><code class="sourceCode php">.recalculatePricesAfter: <span class="dv">60</code></p></td>
</tr>
</tbody>
</table>
