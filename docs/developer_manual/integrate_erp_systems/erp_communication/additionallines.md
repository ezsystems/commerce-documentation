#  AdditionalLines 

## General

When the request for product information is sent to ERP, there is a possibility that in the responce, we will get more information about products that we asked (eg. additional shipping costs, vouchers, another product for free, promotions etc.) 

This addional information is handles by the service which adds the proper lines to basket or basket line.

Additional lines are stored in the basket as $additionalLines\[\].

Additional lines are stored in BasketLine as $assignedLines\[\].

Additional lines will be fetched from Nav or may be calculated locally.

## WebConnectorService

The WebConnectorService passes the whole ERP response object to the [RemotePriceProvider](RemotePriceProvider_23560481.html). The RemotePriceProvider will create additional lines in the price response, if it gets additional price information from ERP that has not been requested.

## AdditionalLine

Defines the class for additional lines that can be related to the basket or basket line. Additional lines can increase or decrease the order sum.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><pre><code>attribute</code></pre></th>
<th><pre><code>type</code></pre></th>
<th><pre><code>usage</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>sku</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>sku or resource number of the cost</code></pre></td>
</tr>
<tr>
<td><pre><code>name</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>name of the cost e.g. &#39;Shipping costs DHL standard&#39;</code></pre></td>
</tr>
<tr>
<td><pre><code>identifier</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>identifier of the cost e.g. &#39;shippingCosts&#39;</code></pre></td>
</tr>
<tr>
<td><pre><code>price</code></pre></td>
<td><pre><code>PriceField</code></pre></td>
<td><pre><code>price of the cost incl. and excl. vat</code></pre></td>
</tr>
<tr>
<td><pre><code>characteristic</code></pre></td>
<td><pre><code>string</code></pre></td>
<td><pre><code>characteristic of the cost</code></pre>
<pre><code>allowed values:</code></pre>
<pre><code>- expense: increase the total amount, has a price</code></pre>
<pre><code>- voucher: decrease the total amount, has a price</code></pre>
<pre><code>- addOn: doesn&#39;t have a price</code></pre></td>
</tr>
</tbody>
</table>

## Storage of additional lines

Additional lines can be stored in Basket and/or BasketLine.

**Example in Template**

``` 
{% for line in basket.additionalLines  %}
    Name: {{ line.name }}
    Price:    {{ ses_render_price(null, line.price,
              {
                'outputPrice': {'property': 'priceInclVat', 'raw' : true}
              }) }}
{% endfor %}
```

## AdditionalLinesServiceInterface

``` 
/**
 * Defines the interface of AdditionalLinesService.
 *
 * This interface describes all service methods provided for the AdditionalLine.
 * This services provides all operations, which are performed on the AdditionalLine,
 * including creating of new additional lines objects, storing them in the basket, or basket line and so on.
 */
interface AdditionalLinesServiceInterface
{
    /**
     * Sets additional lines into the basket and/or basket line, depending on
     * the information in the price response.
     *
     * @param Basket $basket
     * @param PriceResponse $priceResponse
     * @return Basket
     */
    public function setAdditionalLines(Basket $basket, PriceResponse $priceResponse);

    /**
     * sets shipping costs into the basket
     * if $priceRequest is not set, shipping costs are calculated locally
     *
     * @param Basket $basket
     * @param LegacyPriceRequest $priceRequest
     * @return Basket
     * @deprecated use setShippingCosts() instead
     */
    public function setShippingCostsFromLegacy(Basket $basket, LegacyPriceRequest $priceRequest = null);

    /**
     * Sets shipping costs into the basket.
     *
     * @param Basket $basket
     * @param PriceResponse $priceResponse
     * @return Basket
     */
    public function setShippingCosts(Basket $basket, PriceResponse $priceResponse);

    /**
     * Gets shipping costs from the basket.
     *
     * @param Basket $basket
     * @return AdditionalLine[]|null
     */
    public function getShippingCosts(Basket $basket);

    /**
     * Gets discounts from the basket.
     *
     * @param Basket $basket
     * @return AdditionalLine[]|null
     */
    public function getDiscounts(Basket $basket);

    /**
     * Gets additional costs from the basket.
     *
     * @param Basket $basket
     * @return AdditionalLine[]|null
     */
    public function getAddCosts(Basket $basket);

    /**
     * Gets additional lines.
     *
     * @param Basket $basket
     * @return AdditionalLine[]|null
     */
    public function getAddLines(Basket $basket);

    /**
     * Removes the additional line identified by sku from the basket.
     *
     * @param Basket $basket
     * @param string $sku
     * @return Basket
     */
    public function removeAdditionalLine(Basket $basket, $sku);

}
```

## AdditionalLinesService

This service is used to calculate the additional lines and store and fetch them in/from basket/basketLine.

It implements all methods from the AdditionalLinesServiceInterface.

    Service ID:

    siso_basket.additional_lines_service
