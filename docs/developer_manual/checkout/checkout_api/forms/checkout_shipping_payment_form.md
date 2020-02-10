#  Checkout Shipping Payment Form 

## Model Class

This class extends the `AbstractFormEntity` and implements the `CheckoutAddressInterface`.

**Namespace**:

`Siso\Bundle\CheckoutBundle\Form\CheckoutShippingPayment`

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
<td><pre><code>shippingMethod</code></pre></td>
<td>method of the shipping</td>
<td><pre><code>String
Not blank</code></pre></td>
</tr>
<tr>
<td><pre><code>paymentMethod</code></pre></td>
<td>method of the payment</td>
<td><pre><code>String
Not blank</code></pre></td>
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

To learn more about the form configuration, please see here:

  - [Forms Configuration](/pages/createpage.action?spaceKey=EZC14&title=Forms+Configuration&linkCreation=true&fromPageId=23560339)

## Form Type

**Namespace**:

`Siso\Bundle\CheckoutBundle\Form\Type\CheckoutShippingPaymentType`

implements the setup for this form. In this class further definitions are implemented. 

This class is defined as a service in order to take advantage from TransService.

The service definition ID for this form is: `siso_checkout.form_entity.checkout_shipping_payment_type`

Please pay attention, that the scope of this service is set to 'prototype'. Against to default service behavior - a new instance of ` Siso\Bundle\CheckoutBundle\Form\Type\CheckoutShippingPaymentType  `is created every time this service is called.

## Templates

|               |                                                                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Main template | `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_shipping_payment.html.twig` |

Select values configuration

More configuration values are set in the [Configuration for Checkout Forms](Configuration-for-Checkout-Forms_23560355.html).
