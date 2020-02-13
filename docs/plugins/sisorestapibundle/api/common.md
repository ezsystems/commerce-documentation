# Common 

## This page documents general REST API calls:

  - getCountrySelection
  - getCustomerPrice

## getCountrySelection

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/country-selection (GET)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>returns the list of configured country codes and their translation for current siteaccess</p></td>
</tr>
<tr class="odd">
<td>Service ID</td>
<td><p>siso_rest_api.country_selection_service</p>
<p>Inside this service the countries will be fetched from service: siso_tools.country (see <a href="Country-Service_29819029.html">Country Service</a>)</p>
<p>parameter ses_forms.default.preferred_country is used for defaultCountry</p></td>
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
<pre class="" data-syntaxhighlighter-params="brush: js; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;CountrySelectionResponse&quot;: {
        &quot;_media-type&quot;: &quot;application\/vnd.ez.api.CountrySelectionResponse+json&quot;,
        &quot;countryOptions&quot;: {
            &quot;DZ&quot;: &quot;Algerien&quot;,
            &quot;AU&quot;: &quot;Australien&quot;,
            &quot;BE&quot;: &quot;Belgien&quot;,
            &quot;BR&quot;: &quot;Brasilien&quot;,
            &quot;BG&quot;: &quot;Bulgarien&quot;,
            &quot;DK&quot;: &quot;D\u00e4nemark&quot;,
            &quot;DE&quot;: &quot;Deutschland&quot;,
            &quot;EE&quot;: &quot;Estland&quot;,
            &quot;FI&quot;: &quot;Finnland&quot;,
            &quot;FR&quot;: &quot;Frankreich&quot;,
            &quot;GR&quot;: &quot;Griechenland&quot;,
            &quot;IN&quot;: &quot;Indien&quot;,
            &quot;ID&quot;: &quot;Indonesien&quot;,
            &quot;IE&quot;: &quot;Irland&quot;,
            &quot;IS&quot;: &quot;Island&quot;,
            &quot;IT&quot;: &quot;Italien&quot;,
            &quot;CA&quot;: &quot;Kanada&quot;,
            &quot;KE&quot;: &quot;Kenia&quot;,
            &quot;LV&quot;: &quot;Lettland&quot;,
            &quot;LT&quot;: &quot;Litauen&quot;,
            &quot;MY&quot;: &quot;Malaysia&quot;,
            &quot;MT&quot;: &quot;Malta&quot;,
            &quot;MX&quot;: &quot;Mexiko&quot;,
            &quot;MZ&quot;: &quot;Mosambik&quot;,
            &quot;NZ&quot;: &quot;Neuseeland&quot;,
            &quot;NL&quot;: &quot;Niederlande&quot;,
            &quot;NG&quot;: &quot;Nigeria&quot;,
            &quot;NO&quot;: &quot;Norwegen&quot;,
            &quot;AT&quot;: &quot;\u00d6sterreich&quot;,
            &quot;PH&quot;: &quot;Philippinen&quot;,
            &quot;PL&quot;: &quot;Polen&quot;,
            &quot;PT&quot;: &quot;Portugal&quot;,
            &quot;RO&quot;: &quot;Rum\u00e4nien&quot;,
            &quot;RU&quot;: &quot;Russland&quot;,
            &quot;SE&quot;: &quot;Schweden&quot;,
            &quot;CH&quot;: &quot;Schweiz&quot;,
            &quot;SG&quot;: &quot;Singapur&quot;,
            &quot;SK&quot;: &quot;Slowakei&quot;,
            &quot;SI&quot;: &quot;Slowenien&quot;,
            &quot;MO&quot;: &quot;Sonderverwaltungsregion Macau&quot;,
            &quot;ES&quot;: &quot;Spanien&quot;,
            &quot;ZA&quot;: &quot;S\u00fcdafrika&quot;,
            &quot;SZ&quot;: &quot;Swasiland&quot;,
            &quot;TZ&quot;: &quot;Tansania&quot;,
            &quot;TH&quot;: &quot;Thailand&quot;,
            &quot;CZ&quot;: &quot;Tschechische Republik&quot;,
            &quot;TN&quot;: &quot;Tunesien&quot;,
            &quot;TR&quot;: &quot;T\u00fcrkei&quot;,
            &quot;UG&quot;: &quot;Uganda&quot;,
            &quot;HU&quot;: &quot;Ungarn&quot;,
            &quot;AE&quot;: &quot;Vereinigte Arabische Emirate&quot;,
            &quot;US&quot;: &quot;Vereinigte Staaten&quot;,
            &quot;GB&quot;: &quot;Vereinigtes K\u00f6nigreich&quot;,
            &quot;CY&quot;: &quot;Zypern&quot;
        },
        &quot;defaultCountry&quot;: &quot;DE&quot;
    }
}</code></pre>
</td>
</tr>
</tbody>
</table>

## getCustomerprice

<table>
<tbody>
<tr class="odd">
<td>Resourcename</td>
<td><pre><code>/api/ezp/v2/siso-rest/customerprice (POST)</code></pre></td>
</tr>
<tr class="even">
<td>Summary</td>
<td><p>returns the customer price</p></td>
</tr>
<tr class="odd">
<td>Service ID</td>
<td><p>???</p></td>
</tr>
<tr class="even">
<td>Request</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;PriceRequest&quot;: {
        &quot;Meta&quot;: {
            &quot;Context&quot;: &quot;product_list&quot;,
            &quot;CatalogElement&quot;: true
        }
        &quot;Items&quot;: [{
            &quot;Sku&quot;: &quot;1234&quot;,
            &quot;Quantity&quot;: 2,
            &quot;UnitOfMeasure&quot;: &quot;ST&quot;,
            &quot;DataMap&quot;: {}
        }]
    }
}</code></pre>
</td>
</tr>
<tr class="odd">
<td>Response</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: js; gutter: false; theme: Confluence" data-theme="Confluence"><code>{
    &quot;PriceResponse&quot;: [{
        &quot;Sku&quot;: &quot;1234&quot;,
        &quot;RequestedQuantity&quot;: 2,
        &quot;RequestedUnitOfMeasure&quot;: &quot;ST&quot;,
        &quot;CustomerPrice&quot;: {
            &quot;Quantity&quot;: 2,
            &quot;UnitOfMeasure&quot;: &quot;ST&quot;,
            &quot;Price&quot;: 1.50,
            &quot;LinePrice&quot;: 3.00,
            &quot;Currency&quot;: &quot;EUR&quot;,
            &quot;PriceFormatted&quot;: &quot;1.50 €&quot;,
            &quot;LinePriceFormatted&quot;: &quot;3.00 €&quot;
        },
        &quot;ListPrice&quot;: {
            &quot;Quantity&quot;: 2,
            &quot;UnitOfMeasure&quot;: &quot;ST&quot;,
            &quot;Price&quot;: 1.5,
            &quot;LinePrice&quot;: 3.00,
            &quot;Currency&quot;: &quot;EUR&quot;,
            &quot;PriceFormatted&quot;: &quot;1.50 €&quot;,
            &quot;LinePriceFormatted&quot;: &quot;3.00 €&quot;
        },
        &quot;Stock&quot;: 32,
        &quot;Message&quot;: &quot;&quot;,
        &quot;DataMap&quot;: {}
    }]
}</code></pre>
</td>
</tr>
</tbody>
</table>

ConfigResolver Variable, ob Stock nach außen übergeben werden soll

Service mit PriceResponse, Sku, Quantity und ermittelt die Verfügbarkeit, Response ist ein Code "red", "green", "yellow", "grey"
