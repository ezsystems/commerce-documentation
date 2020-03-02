# Common

## This page documents general REST API calls:

- getCountrySelection
- getCustomerPrice

## getCountrySelection

### Resourcename

/api/ezp/v2/siso-rest/country-selection (GET)

### Summary

returns the list of configured country codes and their translation for current siteaccess

### Service ID

siso_rest_api.country_selection_service

Inside this service the countries will be fetched from service: siso_tools.country (see <a href="Country-Service_29819029.html">Country Service</a>)

parameter ses_forms.default.preferred_country is used for defaultCountry

### Request

empty

### Response

```
{
    "CountrySelectionResponse": {
        "_media-type": "application\/vnd.ez.api.CountrySelectionResponse+json",
        "countryOptions": {
            "DZ": "Algerien",
            "AU": "Australien",
            "BE": "Belgien",
            "BR": "Brasilien",
            "BG": "Bulgarien",
            "DK": "D\u00e4nemark",
            "DE": "Deutschland",
            "EE": "Estland",
            "FI": "Finnland",
            "FR": "Frankreich",
            "GR": "Griechenland",
            "IN": "Indien",
            "ID": "Indonesien",
            "IE": "Irland",
            "IS": "Island",
            "IT": "Italien",
            "CA": "Kanada",
            "KE": "Kenia",
            "LV": "Lettland",
            "LT": "Litauen",
            "MY": "Malaysia",
            "MT": "Malta",
            "MX": "Mexiko",
            "MZ": "Mosambik",
            "NZ": "Neuseeland",
            "NL": "Niederlande",
            "NG": "Nigeria",
            "NO": "Norwegen",
            "AT": "\u00d6sterreich",
            "PH": "Philippinen",
            "PL": "Polen",
            "PT": "Portugal",
            "RO": "Rum\u00e4nien",
            "RU": "Russland",
            "SE": "Schweden",
            "CH": "Schweiz",
            "SG": "Singapur",
            "SK": "Slowakei",
            "SI": "Slowenien",
            "MO": "Sonderverwaltungsregion Macau",
            "ES": "Spanien",
            "ZA": "S\u00fcdafrika",
            "SZ": "Swasiland",
            "TZ": "Tansania",
            "TH": "Thailand",
            "CZ": "Tschechische Republik",
            "TN": "Tunesien",
            "TR": "T\u00fcrkei",
            "UG": "Uganda",
            "HU": "Ungarn",
            "AE": "Vereinigte Arabische Emirate",
            "US": "Vereinigte Staaten",
            "GB": "Vereinigtes K\u00f6nigreich",
            "CY": "Zypern"
        },
        "defaultCountry": "DE"
    }
}
```

## getCustomerprice

### Resourcename

/api/ezp/v2/siso-rest/customerprice (POST)

### Summary

returns the customer price

### Service ID

### Request

```
{
    "PriceRequest": {
        "Meta": {
            "Context": "product_list",
            "CatalogElement": true
        }
        "Items": [{
            "Sku": "1234",
            "Quantity": 2,
            "UnitOfMeasure": "ST",
            "DataMap": {}
        }]
    }
}
```

### Response

```
{
    "PriceResponse": [{
        "Sku": "1234",
        "RequestedQuantity": 2,
        "RequestedUnitOfMeasure": "ST",
        "CustomerPrice": {
            "Quantity": 2,
            "UnitOfMeasure": "ST",
            "Price": 1.50,
            "LinePrice": 3.00,
            "Currency": "EUR",
            "PriceFormatted": "1.50 €",
            "LinePriceFormatted": "3.00 €"
        },
        "ListPrice": {
            "Quantity": 2,
            "UnitOfMeasure": "ST",
            "Price": 1.5,
            "LinePrice": 3.00,
            "Currency": "EUR",
            "PriceFormatted": "1.50 €",
            "LinePriceFormatted": "3.00 €"
        },
        "Stock": 32,
        "Message": "",
        "DataMap": {}
    }]
}
```

ConfigResolver Variable, ob Stock nach außen übergeben werden soll

Service mit PriceResponse, Sku, Quantity und ermittelt die Verfügbarkeit, Response ist ein Code "red", "green", "yellow", "grey"
