# Country Service

The purpose of this service is to return a list of countries with corresponding country codes. The country names are translated for every siteaccess. This list of countries is used in all forms in the shop to be displayed as list of choices.

Because this service has knowledge about the form type name, it is possible to return different countries for every form. So it is possible to display different list for the checkout process and different list for the registration process.  

!!! note "CountryService"

    Namespace: Siso/Bundle/ToolsBundle/Service

    Service-ID: siso_tools.country

    Interface: Siso/Bundle/ToolsBundle/Service/CountryServiceInterface

## Configuration

The list of possible countries to choice can be configued in:

``` 
siso_tools.default.countries
```

## Methods

### User Components

Method: getCountryNames

Parameter:

`$formTypeName = null,`

`$locale = null`

Returns:

`string[]`

Description: 

Returns an array of country codes and names
The country name is translated for required Locale $locale. If $locale is not set, current Locale is used.
This function can be customized depending on given $formTypeName, e.g.:

```
array (
    'AF' => 'Afghanistan',
    ...
    'DE' => 'Germany'
)
```
