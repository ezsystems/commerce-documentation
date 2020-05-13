# StandardTemplateDebitorService

## Goal

This service can be used when customer doesn't have customer or contact number yet, but such a number is necessary when communicating data to ERP. An example is price calculation, when prices shall be calculated using ERP.

The ERP system is using a concept called Template Debitors.

This service makes usage of [StandardCountryZoneService](standardcountryzoneservice.md) to get the correct zone for the country. The country is determined from the given BuyerParty.

Also customer groups from BuyerParty are considered when determining the template debitor information.

### How the customer groups are stored inside the BuyerParty

``` xml
<Party ses_unbounded="PartyIdentification PartyName" ses_type="ses:Contact" ses_tree="SesExtension">
    <SesExtension>
        <CustomerGroups>
            <Code>GROUPA</Code>
            <Code>GROUPB</Code>
        </CustomerGroups>
    </SesExtension>
</Party> 
```

## Configuration

Service ID:

``` 
 siso_core.template_debitor.standard
```

Default template customer information:

``` yaml
siso_core.template_debitor.customer_numbers:
    #configuration for the zone
    EU:
        #configuration for the country Germany
        DE:
            #customer groups possible:
            #GROUPA: 10001
            #more groups
            #GROUPB: 10002
            #default if no customer groups available
            default: 10000
        #default value for the zone, if no country is specified
        default: 40000
    World:
        default: 20000
    #default value if no zone is specified
    default: 10000

siso_core.template_debitor.contact_numbers:
    #configuration for the zone
    EU:
        #configuration for the country
        DE:
            #customer groups possible:
            #GROUPA: KT100211
            #more groups
            #GROUPB: 10002
            #default if no customer groups available
            default: KT100210
        #default value for the zone, if no country is specified
        default: KT000004
    World:
        default: KT000136
    #default value if no zone is specified
    default: KT100210
```

# API: TemplateDebitorServiceInterface

|Method|Description|
|--- |--- |
|public function getTemplateCustomerNumber(</br> Party $buyerParty = null,</br> Party $invoiceParty = null,</br> Party $deliveryParty = null</br>);|This method will return template customer number depending on the given parties.</br>If no parties are provided, configuration shall be used to provide fallback template customer number.|
|public function getTemplateContactNumber(</br> Party $buyerParty = null,</br> Party $invoiceParty = null,</br> Party $deliveryParty = null</br>);|This method will return template contact number depending on the given parties.</br>If no parties are provided, configuration shall be used to provide fallback template contact number.|
