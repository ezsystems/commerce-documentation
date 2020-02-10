#  Ez5ScaledPriceService 

## Introduction

This service determines the correct price by comparing base price from PriceRequest to '**scaledPrices'** array stored in extended data (also in PriceRequest).The service determines the best price if more scaled prices matches.

This data may e.g come from eZ backend. It is the task of the CatalogFactory to store data in the correct format in the catalog element.

The scaled prices can be setup per product in the backend of the shop:

![](attachments/23560420/23563824.png)

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Field</th>
<th>Identifier</th>
<th>required</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>Customer Group</td>
<td><pre><code>customerGroup</code></pre></td>
<td>no</td>
<td>Code for the customer group</td>
<td>GROUPA</td>
</tr>
<tr>
<td>Minimum Quantity</td>
<td><pre><code>minQuantity</code></pre></td>
<td>yes</td>
<td><p>Minimum quantity for the given price.</p>
<p>If more than one scaled price entries will match the latest matchin entry will be used</p></td>
<td>2</td>
</tr>
<tr>
<td>Start Date</td>
<td><pre><code>startDate</code></pre></td>
<td>yes (can be empty)</td>
<td><p>start date can be set in two formats (date with and without precise hour):</p>
<p>If no time is given then "00:00:00" (hour-minutes-seconds) will be used.</p>
<p>2015-06-01 becomes 2015-06-01 00:00:00</p></td>
<td>2015-06-01 00:00</td>
</tr>
<tr>
<td>End Date</td>
<td><pre><code>endDate</code></pre></td>
<td>no</td>
<td><p>end date can be set in two formats (date with and without precise hour):</p>
<p>If no time is given then "23:59:59" (hour-minutes-seconds) will be used.</p>
<p>2015-06-01 becomes 2015-06-01 23:59:59</p></td>
<td>2015-06-01 22:00</td>
</tr>
<tr>
<td>Price</td>
<td><pre><code>price</code></pre></td>
<td>yes</td>
<td><p>Price used to calculate price gross and price net value.</p>
<p>Price can be with or without VAT</p></td>
<td>12.65</td>
</tr>
<tr>
<td><p>Is Including VAT</p></td>
<td><pre><code>isInclVat</code></pre></td>
<td>yes</td>
<td>value that determines if Price includes VAT or not.</td>
<td>0 or 1</td>
</tr>
<tr>
<td>Price Gross</td>
<td><pre><code>priceGross</code></pre></td>
<td>no</td>
<td><p>if not set then it's calculated based on two previous values: Price and Is Including VAT.</p>
<p>if set price gross value overrides the calculation.</p></td>
<td> </td>
</tr>
<tr>
<td><p>Price Net</p></td>
<td><pre><code>priceNet</code></pre></td>
<td>no</td>
<td><p>if not set then it's calculated based on two previous values: Price and Is Including VAT.</p>
<p>if set price net value overrides the calculation.</p></td>
<td> </td>
</tr>
</tbody>
</table>

ProductNode contains additional attribute for scaledPrices. Ez5CatalogFactory is extracting this value and putting it into an ArrayField.

``` 
* @property-read ArrayField $scaledPrices

Example:
$scaledPrices = array();
foreach($prices as $price) {
    $scaledPrices[] = array(
        'price' => $price['Price'],
        'isInclVat' => true,
        'minQuantity' => $price['MinQty'],
        'startDate' => '',
        'endDate' => ''
    );
}

return new ArrayField(
    array(
        'array' => $scaledPrices
    )
);
```

#### The scaled price matches in cases below:

  - if customer group is not defined or customer group matches the customer group from byuer party

  - if price line quantity is greater than scaled price minimum quantity

  - if startDate is not defined or current date is more than startDate (if defined)

  - if endDate is not defined or current date is less than endDate (if defined)

#### Where the customerGroups are stored ?

In Customer Profile Data inside Buyer Party:

**BuyerParty**

``` 
<Party ses_unbounded="PartyIdentification PartyName" ses_type="ses:Contact" ses_tree="SesExtension">
    <SesExtension>
        <CustomerGroups>
            <Code>GROUPA</Code>
            <Code>GROUPB</Code>
        </CustomerGroups>
    </SesExtension>
</Party> 
```

``` 
array( 'CustomerGroups' => array (
    0 => array('Code' => 'GROUPA'),
    1 => array('Code' => 'GROUPB'),
));
```

## Attachments:

![](images/icons/bullet_blue.gif) [scaled prices ez.jpg](attachments/23560420/23563823.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-06-17 10:36:51.png](attachments/23560420/23563824.png) (image/png)  
