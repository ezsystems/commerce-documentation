#  Checkout Delivery Address Form 

## Model class

This class extends the `AbstractFormEntity` and implements the `CheckoutAddressInterface`

Namespace

`Siso\Bundle\CheckoutBundle\Form\CheckoutDeliveryAddress`

This document describes the form for choosing delivery address in checkout process.

## Fields

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Description</th>
<th>Assertations</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>addressStatus</code></pre></td>
<td>status of delivery address</td>
<td> </td>
</tr>
<tr>
<td><pre><code>company</code></pre></td>
<td>user name or company</td>
<td><pre><code>Not Blank</code></pre>
<pre><code>min = 2</code></pre>
<pre><code>max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>companySecond</code></pre></td>
<td>user second name or company</td>
<td><pre><code>min = 2</code></pre>
<pre><code>max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>street</code></pre></td>
<td>street of the user</td>
<td><pre><code>Not Blank</code></pre>
<pre><code>min = 2</code></pre>
<pre><code>max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>addressSecond</code></pre></td>
<td>optional second address of the user</td>
<td><pre><code>min = 2</code></pre>
<pre><code>max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>zip</code></pre></td>
<td>zip number of the user</td>
<td><pre><code>Not Blank (excluding Ireland)</code></pre>
<pre><code>min = 3</code></pre>
<pre><code>max = 20</code></pre>
<pre><code>Numeric</code></pre></td>
</tr>
<tr>
<td><pre><code>city</code></pre></td>
<td>city of the user</td>
<td><pre><code>Not Blank</code></pre>
<pre><code>min = 2</code></pre>
<pre><code>max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>country</code></pre></td>
<td>country of the user</td>
<td><pre><code>Not Blank</code></pre></td>
</tr>
<tr>
<td><pre><code>county</code></pre></td>
<td>county of the user</td>
<td><pre><code>min = 2</code></pre>
<pre><code>max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>phone</code></pre></td>
<td>phone number of the user</td>
<td><pre><code>SesAssert\Phone</code></pre></td>
</tr>
<tr>
<td><pre><code>email</code></pre></td>
<td>email address of the user</td>
<td><pre><code>SesAssert\Email</code></pre></td>
</tr>
<tr>
<td><pre><code>saveAddress</code></pre></td>
<td>true if user wants to store this address<br />
in his address list</td>
<td>boolean</td>
</tr>
<tr>
<td><pre><code>partyId</code></pre></td>
<td>Id of the party ID in ERP system</td>
<td>string</td>
</tr>
<tr>
<td><pre><code>forceStep</code></pre></td>
<td>True if user wants to force to next step with event errors</td>
<td><pre><code>Boolean</code></pre></td>
</tr>
</tbody>
</table>

## Form Type

Namespace

`Siso\Bundle\CheckoutBundle\Form\Type\CheckoutDeliveryAddressType`

implements the setup for this form. In this class further definitions are implemented.

This class is defined as a service in order to take advantage from other services, like TransService and to be able to read the configuration settings.

The service definition ID for this form is: `siso_checkout.form_entity.checkout_delivery_address_type`

Please pay attention, that the scope of this service is set to 'prototype'. Against to default service behavior - a new instance of `Siso\Bundle\CheckoutBundle\Form\Type\``CheckoutDeliveryAddressType` is created every time this service is called.

##  Configuration

see [Checkout Forms Configuration](Configuration-for-Checkout-Forms_23560355.html).

## Templates

|                              |                                                                                                                                            |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Main template                | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_delivery_address.html.twig` |
| Sidebar template for invoice | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/sidebar_delivery_address.html.twig`  |

## General logic

Error rendering macro 'excerpt-include' : No link could be created for 'INTERNAL:Detail concept delivery address box'.

### Exceptions in validation process for delivery

In some cases we need to suppress the form validation

  - if user has customer number and uses invoice as delivery
  - if user has customer number and uses address from a list. In these cases the data is coming from ERP and user can not change it.
