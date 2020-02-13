# Customer functions 

On this page you will find REST API calls for customer data:

  - getShippingAddresses

## getShippingAddresses

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/customer/addresses/shipping (GET)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>returns available delivery addresses for current customer</p></td>
</tr>
<tr class="odd">
<td>Service</td>
<td><p>Use standard service to get shipping addresses: 'ses.customer_profile_data.ez_erp' </p>
<p>(getCustomerProfileData()-&gt;deliveryParties)</p></td>
</tr>
<tr class="even">
<td>Request</td>
<td><div class="content-wrapper">
<p>empty</p>
</td>
</tr>
<tr class="odd">
<td>Response</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;ShippingAddressesResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.ShippingAddressesResponse+json&quot;,
        &quot;Parties&quot;: [
            {
                &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Party+json&quot;,
                &quot;PartyIdentification&quot;: [],
                &quot;PartyName&quot;: [],
                &quot;PostalAddress&quot;: {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPostalAddress+json&quot;,
                    &quot;StreetName&quot;: null,
                    &quot;AdditionalStreetName&quot;: null,
                    &quot;BuildingNumber&quot;: null,
                    &quot;CityName&quot;: null,
                    &quot;PostalZone&quot;: null,
                    &quot;CountrySubentity&quot;: null,
                    &quot;CountrySubentityCode&quot;: null,
                    &quot;AddressLine&quot;: [],
                    &quot;Country&quot;: {
                        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPostalAddressCountry+json&quot;,
                        &quot;IdentificationCode&quot;: null,
                        &quot;Name&quot;: null
                    },
                    &quot;Department&quot;: null,
                    &quot;SesExtension&quot;: {}
                },
                &quot;Contact&quot;: {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.Contact+json&quot;,
                    &quot;ID&quot;: null,
                    &quot;Name&quot;: null,
                    &quot;Telephone&quot;: null,
                    &quot;Telefax&quot;: null,
                    &quot;ElectronicMail&quot;: null,
                    &quot;OtherCommunication&quot;: null,
                    &quot;Note&quot;: null,
                    &quot;SesExtension&quot;: {}
                },
                &quot;Person&quot;: {
                    &quot;_media-type&quot;: &quot;application\/vnd.ez.api.PartyPerson+json&quot;,
                    &quot;FirstName&quot;: null,
                    &quot;FamilyName&quot;: null,
                    &quot;Title&quot;: null,
                    &quot;MiddleName&quot;: null,
                    &quot;SesExtension&quot;: {}
                },
                &quot;SesExtension&quot;: {}
            }
        ]
    }
}</code></pre>
</td>
</tr>
</tbody>
</table>
