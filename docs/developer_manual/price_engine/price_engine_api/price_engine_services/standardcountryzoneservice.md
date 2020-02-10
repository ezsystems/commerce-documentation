#  StandardCountryZoneService 

## Goal

The advantage of this service is to use it as a helper to resolve settings based on a region or zone.  It uses simple configuration to determine the zone by given country.

It is used e.g. for selecting template debitors in the [StandardTemplateDebitorService](StandardTemplateDebitorService_23560368.html).

## Configuration

**Service ID**

``` 
siso_core.country_zone.standard
```

**Default zones configuration**

``` 
#values for the siso_core.country_zone.standard service
silver_eshop.zone_country:
    EU: ['AT', 'BE', 'BG', 'CY', 'CZ', 'DE', 'DK', 'EE', 'EL', 'ES', 'FI', 'FR', 'GB', 'HR', 'HU', 'IE', 'IT', 'LT', 'LU', 'LV', 'MT', 'NL', 'PL', 'PT', 'RO', 'SE', 'SI', 'SK']
```

## API: CountryZoneServiceInterface

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Method</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>public function getCountries($zone);</code></pre></td>
<td><pre><code>This method will return a list of countries for given zone</code></pre></td>
</tr>
<tr>
<td><pre><code>public function getZone($country);</code></pre></td>
<td><pre><code>This method will return zone for given country</code></pre></td>
</tr>
</tbody>
</table>
