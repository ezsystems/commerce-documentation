#  LocalVatService 

## Introduction

This service can be used as a fallback if PriceRequest does not provide the VAT value. Typically the ERP can provide the VAT in percent during a price request. 

If not provided the vat code stored in the ProductNode can be used to determine the current Vat in percent using a configuration.

The configuration considers 

  - siteaccess
  - country code  (this might be the country code for the buyer or in some cases the country code of the delivery destination). If the country was not found in the siteaccess configuration, 'default' is used as a fallback. In such a case an entry is added into log file.  
    
  - VAT code.

Because of legal reasons, if no VAT value is found exception will be thrown. We cannot display default VAT percent in this case.

In our standard implementation the VAT code for calculation is provided by ProductNode attribute vatCode (catalogElement-\>vatCode).

## Configuration

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/silver.eshop.yml**

``` 
// if there is no information for country code in the priceRequest use fallback
siso_core.default.standard_price_factory.fallback_country: DE

// vat configuration (siteaccess, country code and vat code)
siso_core.engl.vat:
    EN:
        vegetable: 7
        print: 7
        #some NAV special vat code naming
        9: 9
    #default is not a country code, but used as a fallback configuration
    default:
        vegetable: 7
        print: 7
        9: 9

siso_core.default.vat:
    DE:
        vegetable: 7
        print: 7
        #some NAV special vat code naming
        9: 9
    #default is not a country code, but used as a fallback configuration
    default:
        vegetable: 7
        print: 7
        9: 9
```
