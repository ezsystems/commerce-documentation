
#  Form Interfaces 

## FormEntityInterface

Namespace

Silversolutions\\Bundle\\EshopBundle\\Entities\\Forms\\FormEntityInterface

FormEntityInterface is an interface for form entities, which defines a common way to access to the entity data.

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
<td><pre><code>get</code></pre></td>
<td><pre><code>$key</code></pre></td>
<td><pre><code>get access to all private attributes, e.g. for the normalizer</code></pre></td>
</tr>
<tr>
<td><pre><code>set</code></pre></td>
<td><pre><code>$key, $value</code></pre></td>
<td><pre><code>get access to set all private attributes, e.g. for the normalizer</code></pre></td>
</tr>
<tr>
<td><pre><code>hasChanged</code></pre></td>
<td> </td>
<td><pre><code>returns true if the form values have been changed after the form was submitted</code></pre></td>
</tr>
<tr>
<td><pre><code>setChanged</code></pre></td>
<td><pre><code>bool $changed</code></pre></td>
<td><pre><code>sets the form changed flag</code></pre></td>
</tr>
</tbody>
</table>

## CheckoutFormInterface

Namespace

Siso\\Bundle\\CheckoutBundle\\Api\\CheckoutFormInterface

Provide methods that are necessary for all checkout forms.

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
<td><pre><code>setForceStep</code></pre></td>
<td><pre><code>$forceStep</code></pre></td>
<td><pre><code>sets the force step value</code></pre></td>
</tr>
<tr>
<td><pre><code>getForceStep</code></pre></td>
<td> </td>
<td><pre><code>gets the force step value</code></pre></td>
</tr>
</tbody>
</table>

## CheckoutAddressInterface

Namespace

Silversolutions\\Bundle\\EshopBundle\\Form\\CheckoutAddressInterface

Provide methods that are necessary for forms that are handling addresses in checkout process.

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
<td><pre><code>setAddressSecond</code></pre></td>
<td><pre><code>$addressSecond</code></pre></td>
<td><pre><code>sets the address second</code></pre></td>
</tr>
<tr>
<td><pre><code>getAddressSecond</code></pre></td>
<td><pre><code> </code></pre></td>
<td><pre><code>gets the address second</code></pre></td>
</tr>
<tr>
<td><pre><code>setCity</code></pre></td>
<td><pre><code>$city</code></pre></td>
<td><pre><code>sets the city</code></pre></td>
</tr>
<tr>
<td><pre><code>getCity</code></pre></td>
<td> </td>
<td><pre><code>gets the city</code></pre></td>
</tr>
<tr>
<td><pre><code>setCompany</code></pre></td>
<td><pre><code>$company</code></pre></td>
<td><pre><code>sets the company</code></pre></td>
</tr>
<tr>
<td><pre><code>getCompany</code></pre></td>
<td> </td>
<td><pre><code>gets the company</code></pre></td>
</tr>
<tr>
<td><pre><code>setCountry</code></pre></td>
<td><pre><code>$country</code></pre></td>
<td><pre><code>sets the country</code></pre></td>
</tr>
<tr>
<td><pre><code>getCountry</code></pre></td>
<td> </td>
<td><pre><code>gets the country</code></pre></td>
</tr>
<tr>
<td><pre><code>setCounty</code></pre></td>
<td><pre><code>$county</code></pre></td>
<td><pre><code>sets the county</code></pre></td>
</tr>
<tr>
<td><pre><code>getCounty</code></pre></td>
<td> </td>
<td><pre><code>gets the county</code></pre></td>
</tr>
<tr>
<td><pre><code>setEmail</code></pre></td>
<td><pre><code>$email</code></pre></td>
<td><pre><code>sets the email address</code></pre></td>
</tr>
<tr>
<td><pre><code>getEmail</code></pre></td>
<td> </td>
<td><pre><code>gets the email address</code></pre></td>
</tr>
<tr>
<td><pre><code>setCompanySecond</code></pre></td>
<td><pre><code>$name</code></pre></td>
<td><pre><code>sets the company second</code></pre></td>
</tr>
<tr>
<td><pre><code>getCompanySecond</code></pre></td>
<td> </td>
<td><pre><code>gets the company second</code></pre></td>
</tr>
<tr>
<td><pre><code>setPhone</code></pre></td>
<td><pre><code>$phone</code></pre></td>
<td><pre><code>sets the phone number</code></pre></td>
</tr>
<tr>
<td><pre><code>getPhone</code></pre></td>
<td> </td>
<td><pre><code>gets the phone number</code></pre></td>
</tr>
<tr>
<td><pre><code>setStreet</code></pre></td>
<td><pre><code>$street</code></pre></td>
<td><pre><code>sets the street</code></pre></td>
</tr>
<tr>
<td><pre><code>getStreet</code></pre></td>
<td> </td>
<td><pre><code>gets the street</code></pre></td>
</tr>
<tr>
<td><pre><code>setZip</code></pre></td>
<td><pre><code>$zip</code></pre></td>
<td><pre><code>sets the zip number</code></pre></td>
</tr>
<tr>
<td><pre><code>getZip</code></pre></td>
<td> </td>
<td><pre><code>gets the zip number</code></pre></td>
</tr>
</tbody>
</table>
