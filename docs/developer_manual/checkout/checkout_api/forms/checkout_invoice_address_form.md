#  Checkout Invoice Address Form 

## Model Class

This class extends the `AbstractFormEntity` and implements the `CheckoutAddressInterface`.

**Namespace**:

`Siso\Bundle\CheckoutBundle\Form\CheckoutInvoiceAddress`

## Input Fields

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><pre><code>Name</code></pre></th>
<th><pre><code>Description</code></pre></th>
<th><pre><code>Assertations</code></pre></th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>company</code></pre></td>
<td>name or company of the user</td>
<td><pre><code>Not blank
min = 2
max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>companySecond</code></pre></td>
<td>second name or company of the user</td>
<td><pre><code>min = 2
max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>street</code></pre></td>
<td>street of the company</td>
<td><pre><code>String</code></pre>
Not blank<br />
min = 2<br />
max = 30</td>
</tr>
<tr>
<td><pre><code>addressSecond</code></pre></td>
<td>second address of the user</td>
<td><pre><code>String
min = 2
max = 30</code></pre></td>
</tr>
<tr>
<td><pre><code>zip</code></pre></td>
<td>zip of the user</td>
<td><pre><code>String
Not blank (excluding ireland)
min=2
max=30</code></pre></td>
</tr>
<tr>
<td><pre><code>city</code></pre></td>
<td>city of the user</td>
<td><pre><code>String
Not blank
min=2
max=30</code></pre></td>
</tr>
<tr>
<td><pre><code>country</code></pre></td>
<td>country of the user</td>
<td><pre><code>String
Not blank</code></pre></td>
</tr>
<tr>
<td><pre><code>county</code></pre></td>
<td>county of the user</td>
<td><pre><code>String
min=2
max=30</code></pre></td>
</tr>
<tr>
<td><pre><code>phone</code></pre></td>
<td>phone number of the user</td>
<td><pre><code>SesAssert\Phone</code></pre></td>
</tr>
<tr>
<td><pre><code>email</code></pre></td>
<td>email of the user</td>
<td><pre><code>Not blank
SesAssert\Email</code></pre></td>
</tr>
<tr>
<td><pre><code>invoiceSameAsDelivery</code></pre></td>
<td>True if user wants to use this address as delivery address</td>
<td><pre><code>Boolean</code></pre></td>
</tr>
<tr>
<td><pre><code>forceStep</code></pre></td>
<td>True if user wants to force to next step with event errors</td>
<td><pre><code>Boolean</code></pre></td>
</tr>
</tbody>
</table>

## Configuration

The parameters are set in the [Configuration for Checkout Forms](Configuration-for-Checkout-Forms_23560355.html).

To lean more about the form configuration, please see here:

  - [Forms Configuration](/pages/createpage.action?spaceKey=EZC14&title=Forms+Configuration&linkCreation=true&fromPageId=23560360)

## Form Type

**Namespace**:

`Siso\Bundle\CheckoutBundle\Form\Type\CheckoutInvoiceAddressType`

implements the setup for this form. In this class further definitions are implemented. 

This class is defined as a service in order to take advantage from other services, like TransService and to be able to read the configuration settings.

The service definition ID for this form is: `siso_checkout.form_entity.checkout_invoice_address_type`

Please pay attention, that the scope of this service is set to 'prototype'. Against to default service behavior - a new instance of `Siso\Bundle\CheckoutBundle\Form\Type\CheckoutInvoiceAddressType` is created every time this service is called.

## Templates

|                              |                                                                                                                                           |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Main template                | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_invoice_address.html.twig` |
| Sidebar template for invoice | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/sidebar_invoice_address.html.twig`  |

### Exceptions in validation process for invoice

In some cases we need to suppress the form validation

  - if user has customer number
