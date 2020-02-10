# StandardTemplateDebitorService 

## Goal

This service can be used when customer doesn´t have customer or contact number yet, but such a number is necessary when communicating data to ERP. An example is price calculation, when prices shall be calculated using ERP.

The ERP system is using a concept called [*Template Debitors*](Term---Template-Debitor_23560379.html).

This service makes usage of [StandardCountryZoneService](StandardCountryZoneService_23560363.html) to get the correct zone for the country. The country is determined from the given *[BuyerParty](Term---BuyerParty_23560433.html).*

Also customer groups from BuyerParty are considered when determining the template debitor informations.

### How the customer groups are stored inside the BuyerParty

``` 
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

**Service ID**

``` 
 siso_core.template_debitor.standard
```

**Default template customer information**

``` 
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

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Method
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>public function getTemplateCustomerNumber(
 Party $buyerParty = null,
 Party $invoiceParty = null,
 Party $deliveryParty = null
);</code></pre></td>
<td><pre><code>This method will return template customer number depending on the given parties.
If no parties are provided, configuration shall be used to provide fallback template customer number.</code></pre></td>
</tr>
<tr>
<td><pre><code>public function getTemplateContactNumber(
 Party $buyerParty = null,
 Party $invoiceParty = null,
 Party $deliveryParty = null
);</code></pre></td>
<td><pre><code>This method will return template contact number depending on the given parties.
If no parties are provided, configuration shall be used to provide fallback template contact number.</code></pre></td>
</tr>
</tbody>
</table>
