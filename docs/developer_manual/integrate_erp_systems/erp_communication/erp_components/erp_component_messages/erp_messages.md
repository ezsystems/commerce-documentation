#  ERP Messages: ReadDeliveryAddress, UpdateDeliveryAddress, CreateDeliveryAddress, DeleteDeliveryAddress 

This document presents the technical documentation of the [CRUD](https://de.wikipedia.org/wiki/CRUD) messages for delivery or shipping addresses. All messages are structural very similar. For Read and Delete, the ERP system is likely to expect only the PartyIdentification in the request. The Delete response only contains a status response as text.

# ReadDeliveryAddress

[See: Reusable DeliveryParty element](#ERPMessages:ReadDeliveryAddress,UpdateDeliveryAddress,CreateDeliveryAddress,DeleteDeliveryAddress-ReusableDeliveryPartyelement)

## Request

``` 
<ReadDeliveryAddressRequest ses_type="ses:DeliveryParty">
    <DeliveryParty />
</ReadDeliveryAddressRequest> 
```

## Response

``` 
<ReadDeliveryAddressResponse ses_type="ses:DeliveryParty">
    <DeliveryParty />
</ReadDeliveryAddressResponse>
```

# UpdateDeliveryAddress

[See: Reusable DeliveryParty element](#ERPMessages:ReadDeliveryAddress,UpdateDeliveryAddress,CreateDeliveryAddress,DeleteDeliveryAddress-ReusableDeliveryPartyelement)

## Request

``` 
<UpdateDeliveryAddressRequest ses_type="ses:DeliveryParty">
    <DeliveryParty />
</UpdateDeliveryAddressRequest>
```

## Response

``` 
<UpdateDeliveryAddressResponse ses_type="ses:DeliveryParty">
    <DeliveryParty />
</UpdateDeliveryAddressResponse>
```

# CreateDeliveryAddress

[See: Reusable DeliveryParty element](#ERPMessages:ReadDeliveryAddress,UpdateDeliveryAddress,CreateDeliveryAddress,DeleteDeliveryAddress-ReusableDeliveryPartyelement)

## Request

``` 
<CreateDeliveryAddressRequest ses_type="ses:DeliveryParty">
    <DeliveryParty />
</CreateDeliveryAddressRequest>
```

## Response

``` 
<CreateDeliveryAddressResponse ses_type="ses:DeliveryParty">
    <DeliveryParty />
</CreateDeliveryAddressResponse> 
```

# DeleteDeliveryAddress

[See: Reusable DeliveryParty element](#ERPMessages:ReadDeliveryAddress,UpdateDeliveryAddress,CreateDeliveryAddress,DeleteDeliveryAddress-ReusableDeliveryPartyelement)

## Request

``` 
<DeleteDeliveryAddressRequest ses_type="ses:DeliveryParty">
    <DeliveryParty />
</DeleteDeliveryAddressRequest>
```

## Response

``` 
<DeleteDeliveryAddressResponse>
    <Result />
</DeleteDeliveryAddressResponse>
```

# Reusable DeliveryParty element

``` 
<?xml version="1.0" encoding="UTF-8"?>
<DeliveryParty ses_unbounded="PartyIdentification PartyName" ses_type="ses:Contact" ses_tree="SesExtension">
    <PartyIdentification>
        <ID>10000</ID>
    </PartyIdentification>
    <PartyName>
        <Name>Möbel-Meller KG</Name>
    </PartyName>
    <PostalAddress ses_unbounded="AddressLine" ses_tree="SesExtension">
        <StreetName>Tischlerstr. 4-10</StreetName>
        <AdditionalStreetName />
        <BuildingNumber>4-10</BuildingNumber>
        <CityName>Berlin</CityName>
        <PostalZone>12555</PostalZone>
        <CountrySubentity>Berlin</CountrySubentity>
        <CountrySubentityCode>BER</CountrySubentityCode>
        <AddressLine>
            <Line>Gartenhaus</Line>
        </AddressLine>
        <Country>
            <IdentificationCode>DE</IdentificationCode>
            <Name>Deutschland</Name>
        </Country>
        <Department>Development</Department>
        <SesExtension />
    </PostalAddress>
    <Contact>
    </Contact>
    <Person ses_tree="SesExtension">
        <FirstName>Frank</FirstName>
        <FamilyName>Dege</FamilyName>
        <Title>Herr</Title>
        <MiddleName />
        <SesExtension />
    </Person>
    <SesExtension />
</DeliveryParty> 
```

[See: Reusable Contact element](23560378.html#ERPMessage:SelectCustomer-ReusableContactelement)
