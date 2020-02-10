# Checkout Summary Form

**Namespace**:

`\Siso\Bundle\CheckoutBundle\Form\CheckoutSummary`

This document describes the HTML form for order summary in checkout process.

The order summary form derives from the `AbstractFormEntity`.

## Input Fields

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
<td><code>termsAndConditions</code></td>
<td>"Terms and conditions" Checkbox field</td>
<td><p>Boolean</p>
<p>NotBlank</p></td>
</tr>
<tr>
<td><code>comment</code></td>
<td>Optional user comment/remark</td>
<td><p>String</p>
<p>Length, max = 255</p></td>
</tr>
<tr>
<td><pre><code>forceStep</code></pre></td>
<td>True if user wants to force to next step with event errors</td>
<td><pre><code>Boolean</code></pre></td>
</tr>
</tbody>
</table>

## Configuration

Please see [Configuration for Checkout Forms](Configuration-for-Checkout-Forms_23560355.html).

To learn more about the form configuration, please see here:

  - `Forms Configuration`

## Form Type

**Namespace**:

`\Siso\Bundle\CheckoutBundle\Form\Type\CheckoutSummaryType`

implements the setup for this form. In this class further definitions are implemented. 

This class is defined as a service in order to take advantage from other services, like TransService and to be able to read to configuration settings.

The service definition ID for this form is: `siso_checkout.form_entity.checkout_summary_type`

Please pay attention, that the scope of this service is set to 'prototype'. Against to default service behavior - a new instance of `Siso\Bundle\CheckoutBundle\Form\Type\CheckoutSummaryType` is created every time this service is called.

### Select values configuration

More configuration values are set in the config file [checkout.yml](http://confluence.ng.silverproducts.de/display/EX/Configuration+for+Checkout+Forms).
