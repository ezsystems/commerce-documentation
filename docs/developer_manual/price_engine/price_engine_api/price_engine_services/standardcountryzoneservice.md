# StandardCountryZoneService

## Goal

The advantage of this service is to use it as a helper to resolve settings based on a region or zone.  It uses simple configuration to determine the zone by given country.

It is used e.g. for selecting template debitors in the [StandardTemplateDebitorService](StandardTemplateDebitorService_23560368.html).

## Configuration

Service ID:

``` 
siso_core.country_zone.standard
```

Default zones configuration:

``` 
#values for the siso_core.country_zone.standard service
silver_eshop.zone_country:
    EU: ['AT', 'BE', 'BG', 'CY', 'CZ', 'DE', 'DK', 'EE', 'EL', 'ES', 'FI', 'FR', 'GB', 'HR', 'HU', 'IE', 'IT', 'LT', 'LU', 'LV', 'MT', 'NL', 'PL', 'PT', 'RO', 'SE', 'SI', 'SK']
```

## API: CountryZoneServiceInterface

|Method|Description|
|--- |--- |
|public function getCountries($zone);|This method will return a list of countries for given zone|
|public function getZone($country);|This method will return zone for given country|
