#  Interfaces for checkout services 

## CheckoutFormServiceInterface

**Namespace**

``` 
Silversolutions\Bundle\EshopBundle\Service\CheckoutFormServiceInterface 
```

CheckoutFormServiceInterface is an interface for checkout forms, which defines a common way to prefill the form and store the data in basket.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>storeFormDataInBasket</code></pre></td>
<td><pre><code>FormEntityInterface $form</code></pre>
<pre><code>Basket $basket</code></pre></td>
<td><pre><code>used to persist the form data in basket</code></pre></td>
</tr>
<tr>
<td><pre><code>prefillForm</code></pre></td>
<td><pre><code>FormEntityInterface $form</code></pre>
<pre><code>Basket $basket</code></pre></td>
<td><pre><code>used to prefill the form with some data</code></pre></td>
</tr>
</tbody>
</table>

## CheckoutSummaryFormServiceInterface

**Namespace**

``` 
Silversolutions\Bundle\EshopBundle\Service\CheckoutSummaryFormServiceInterface
```

CheckoutSummaryFormServiceInterface is an interface for checkout summary forms, that handles getting the user confirmation email.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>getCustomerEmailForOrderConfirmation</code></pre></td>
<td><pre><code>Basket $basket</code></pre></td>
<td><pre><code>get the user confirmation email</code></pre></td>
</tr>
<tr>
<td><pre><code>getSalesEmailForOrderConfirmation</code></pre></td>
<td><pre><code>Basket $basket</code></pre></td>
<td><pre><code>get the confirmation email address for the sales contact</code></pre></td>
</tr>
</tbody>
</table>

## CheckoutAddressFormServiceInterface

**Namespace**

``` 
Silversolutions\Bundle\EshopBundle\Service\CheckoutAddressFormServiceInterface 
```

CheckoutAddressFormServiceInterface is an interface for checkout forms, that handles addresses, which defines a way to convert form data into a party and back.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Parameters</th>
<th>Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>convertFormDataToParty</code></pre></td>
<td><pre><code>CheckoutAddressInterface $form</code></pre></td>
<td><pre><code>converts the form data into the Party</code></pre></td>
</tr>
<tr>
<td><pre><code>convertPartyToFormData</code></pre></td>
<td><pre><code>Party $party</code></pre>
<pre><code>CheckoutAddressInterface $form = null</code></pre></td>
<td><pre><code>converts the Party data into the form</code></pre></td>
</tr>
</tbody>
</table>
