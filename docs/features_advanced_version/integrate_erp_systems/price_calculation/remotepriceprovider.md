# RemotePriceProvider

## Goal

The RemotePriceProvider is able to contact an ERP system in order to get prices for one or more products. To request prices from ERP, customer and optional contact number must be provided.

## How customer- and contact numbers are used

1. If the customer- and contact numbers are set directly in the price request, they are used
1. If not, and the customer- and contact numbers are set in the BuyerParty, they are used
1. If neither customer nor contact number are set (as above) and the usage of the template debitor is allowed in the configuration, the customer- and/or contact number are determined from the [StandardTemplateDebitorService](../../../developer_manual/price_engine/price_engine_api/price_engine_services/standardtemplatedebitorservice.md) and used  

Configuration: usage of the template debitor:

``` yaml
siso_core.default.use_template_debitor_customer_number: true
siso_core.default.use_template_debitor_contact_number: true
```

!!! note

    Step 3 will only work, if price requests without customer number are enabled for the `RemotePriceProvider`. It can be enabled/disabled in the Configuration Settings of the shop (Price group).

    While disabled an exception is thrown in the `remotePriceProvider` and fallback will be used.

    `RemotePriceProvider.php`:

    ``` php
    //use the template debitor only if the $customerNumber and $contactNumber are empty!
    if (empty($customerNumber) && empty($contactNumber)) {
        if (!$this->configResolver->getParameter('price_requests_without_customerno', 'siso_price')) {
            throw new PriceCalculationFailedException('No price request is sent without customer number.');
        }

    if ($this->useTemplateDebitorCustomerNumber) {
        $customerNumber = $fallbackCustomerNumber;
    }
    ```

## Request to the ERP/Web-Connector

Webconnector service uses the data from the PriceRequest to build the ERP Request object. The ERP request contains header information such as the customer number and an optional contact number and if applicable the delivery address. Of course, also information about requested products is a part of the request.

#### Customer Number (in this case "10000")

``` 
  <BuyerCustomerParty>
    <SupplierAssignedAccountID>469</SupplierAssignedAccountID>
    <Party>
      <PartyIdentification>
        <ID>10000</ID>
      </PartyIdentification>
      ... 
```

??? note "Delivery information (required for calculating shipping costs)"

    ``` xml
     <Delivery>
        <RequestedDeliveryPeriod>
          <EndDate/>
          <EndTime/>
        </RequestedDeliveryPeriod>
        <DeliveryParty>
          <PartyIdentification>
            <ID/>
          </PartyIdentification>
          <PartyName>
            <Name>Melanie Bourne</Name>
          </PartyName>
          <PartyName>
            <Name/>
          </PartyName>
          <PostalAddress>
            <StreetName>F&#xE4;rberstra&#xDF;e 14</StreetName>
            <AdditionalStreetName/>
            <BuildingNumber/>
            <CityName>Berlin</CityName>
            <PostalZone>12345</PostalZone>
            <CountrySubentity/>
            <CountrySubentityCode/>
            <Country>
              <IdentificationCode>DE</IdentificationCode>
              <Name/>
            </Country>
            <Department/>
            <SesExtension/>
          </PostalAddress>
          <Contact>
            <ID/>
            <Name/>
            <Telephone/>
            <Telefax/>
            <ElectronicMail/>
            <OtherCommunication/>
            <Note/>
            <SesExtension/>
          </Contact>
          <Person>
            <FirstName/>
            <FamilyName/>
            <Title/>
            <MiddleName/>
            <SesExtension/>
          </Person>
          <SesExtension>
            <status>sameAsInvoice</status>
            <store/>
          </SesExtension>
        </DeliveryParty>
      </Delivery>
    ```

??? note "Information about the requested products"

    ``` xml
      <OrderLine>
        <LineItem>
          <ID/>
          <SalesOrderID/>
          <Quantity>1</Quantity>
          <LineExtensionAmount/>
          <TotalTaxAmount/>
          <MinimumQuantity/>
          <MaximumQuantity/>
          <MinimumBackorderQuantity/>
          <MaximumBackorderQuantity/>
          <PartialDeliveryIndicator/>
          <BackOrderAllowedIndicator/>
          <Price>
            <PriceAmount/>
            <BaseQuantity/>
          </Price>
          <Item>
            <Name/>
            <SellersItemIdentification>
              <ID>SE0102</ID>
              <ExtendedID/>
            </SellersItemIdentification>
            <BuyersItemIdentification>
              <ID/>
            </BuyersItemIdentification>
          </Item>
          <SesExtension/>
        </LineItem>
        <SesExtension/>
      </OrderLine>
    ```

??? note "Full request example"

    ``` xml
    <?xml version="1.0"?>
    <Order>
      <DocumentCurrencyCode>EUR</DocumentCurrencyCode>
      <BuyerCustomerParty>
        <SupplierAssignedAccountID>469</SupplierAssignedAccountID>
        <Party>
          <PartyIdentification>
            <ID>10000</ID>
          </PartyIdentification>
          <PartyName>
            <Name>Melanie Bourne</Name>
          </PartyName>
          <PostalAddress>
            <StreetName>F&#xE4;rberstra&#xDF;e 14</StreetName>
            <AdditionalStreetName/>
            <BuildingNumber/>
            <CityName>Berlin</CityName>
            <PostalZone>12345</PostalZone>
            <CountrySubentity/>
            <CountrySubentityCode/>
            <AddressLine>
              <Line/>
            </AddressLine>
            <Country>
              <IdentificationCode>DE</IdentificationCode>
              <Name/>
            </Country>
            <Department/>
            <SesExtension/>
          </PostalAddress>
          <Contact>
            <ID/>
            <Name>Stephanie Bourne</Name>
            <Telephone/>
            <Telefax/>
            <ElectronicMail>mobel-meller.kg@cronuscorp.net</ElectronicMail>
            <OtherCommunication/>
            <Note/>
            <SesExtension/>
          </Contact>
          <Person>
            <FirstName/>
            <FamilyName/>
            <Title/>
            <MiddleName/>
            <SesExtension/>
          </Person>
          <SesExtension>
            <CustomerPostingGroup>INLAND</CustomerPostingGroup>
          </SesExtension>
        </Party>
      </BuyerCustomerParty>
      <SellerSupplierParty>
        <Party>
          <PostalAddress>
            <StreetName/>
            <AdditionalStreetName/>
            <BuildingNumber/>
            <CityName/>
            <PostalZone/>
            <CountrySubentity/>
            <CountrySubentityCode/>
            <Country>
              <IdentificationCode/>
              <Name/>
            </Country>
            <Department/>
            <SesExtension/>
          </PostalAddress>
          <Contact>
            <ID/>
            <Name/>
            <Telephone/>
            <Telefax/>
            <ElectronicMail/>
            <OtherCommunication/>
            <Note/>
            <SesExtension/>
          </Contact>
          <Person>
            <FirstName/>
            <FamilyName/>
            <Title/>
            <MiddleName/>
            <SesExtension/>
          </Person>
          <SesExtension/>
        </Party>
      </SellerSupplierParty>
      <AccountingCustomerParty>
        <Party>
          <PartyIdentification>
            <ID>10000</ID>
          </PartyIdentification>
          <PartyName>
            <Name>Melanie Bourne</Name>
          </PartyName>
          <PostalAddress>
            <StreetName>F&#xE4;rberstra&#xDF;e 14</StreetName>
            <AdditionalStreetName/>
            <BuildingNumber/>
            <CityName>Berlin</CityName>
            <PostalZone>12345</PostalZone>
            <CountrySubentity/>
            <CountrySubentityCode/>
            <AddressLine>
              <Line/>
            </AddressLine>
            <Country>
              <IdentificationCode>DE</IdentificationCode>
              <Name/>
            </Country>
            <Department/>
            <SesExtension/>
          </PostalAddress>
          <Contact>
            <ID/>
            <Name>Stephanie Bourne</Name>
            <Telephone/>
            <Telefax/>
            <ElectronicMail>haendler@silversolutions.de</ElectronicMail>
            <OtherCommunication/>
            <Note/>
            <SesExtension/>
          </Contact>
          <Person>
            <FirstName/>
            <FamilyName/>
            <Title/>
            <MiddleName/>
            <SesExtension/>
          </Person>
          <SesExtension/>
        </Party>
      </AccountingCustomerParty>
      <Delivery>
        <RequestedDeliveryPeriod>
          <EndDate/>
          <EndTime/>
        </RequestedDeliveryPeriod>
        <DeliveryParty>
          <PartyIdentification>
            <ID/>
          </PartyIdentification>
          <PartyName>
            <Name>Melanie Bourne</Name>
          </PartyName>
          <PartyName>
            <Name/>
          </PartyName>
          <PostalAddress>
            <StreetName>F&#xE4;rberstra&#xDF;e 14</StreetName>
            <AdditionalStreetName/>
            <BuildingNumber/>
            <CityName>Berlin</CityName>
            <PostalZone>12345</PostalZone>
            <CountrySubentity/>
            <CountrySubentityCode/>
            <Country>
              <IdentificationCode>DE</IdentificationCode>
              <Name/>
            </Country>
            <Department/>
            <SesExtension/>
          </PostalAddress>
          <Contact>
            <ID/>
            <Name/>
            <Telephone/>
            <Telefax/>
            <ElectronicMail/>
            <OtherCommunication/>
            <Note/>
            <SesExtension/>
          </Contact>
          <Person>
            <FirstName/>
            <FamilyName/>
            <Title/>
            <MiddleName/>
            <SesExtension/>
          </Person>
          <SesExtension>
            <status>sameAsInvoice</status>
            <store/>
          </SesExtension>
        </DeliveryParty>
      </Delivery>
      <PaymentMeans>
        <PaymentMeansCode>invoice</PaymentMeansCode>
        <PaymentDueDate/>
        <PaymentChannelCode/>
        <InstructionID/>
        <PayeeFinancialAccount>
          <ID/>
          <CurrencyCode/>
          <FinancialInstitutionBranch>
            <ID/>
            <Name/>
          </FinancialInstitutionBranch>
        </PayeeFinancialAccount>
        <CardAccount>
          <PrimaryAccountNumberID/>
          <NetworkID/>
          <ExpiryDate/>
        </CardAccount>
      </PaymentMeans>
      <TransactionConditions>
        <ID/>
        <ActionCode/>
      </TransactionConditions>
      <OrderLine>
        <LineItem>
          <ID/>
          <SalesOrderID/>
          <Quantity>1</Quantity>
          <LineExtensionAmount/>
          <TotalTaxAmount/>
          <MinimumQuantity/>
          <MaximumQuantity/>
          <MinimumBackorderQuantity/>
          <MaximumBackorderQuantity/>
          <PartialDeliveryIndicator/>
          <BackOrderAllowedIndicator/>
          <Price>
            <PriceAmount/>
            <BaseQuantity/>
          </Price>
          <Item>
            <Name/>
            <SellersItemIdentification>
              <ID>SE0102</ID>
              <ExtendedID/>
            </SellersItemIdentification>
            <BuyersItemIdentification>
              <ID/>
            </BuyersItemIdentification>
          </Item>
          <SesExtension/>
        </LineItem>
        <SesExtension/>
      </OrderLine>
      <OrderLine>
        <LineItem>
          <ID/>
          <SalesOrderID/>
          <Quantity>1</Quantity>
          <LineExtensionAmount/>
          <TotalTaxAmount/>
          <MinimumQuantity/>
          <MaximumQuantity/>
          <MinimumBackorderQuantity/>
          <MaximumBackorderQuantity/>
          <PartialDeliveryIndicator/>
          <BackOrderAllowedIndicator/>
          <Price>
            <PriceAmount/>
            <BaseQuantity/>
          </Price>
          <Item>
            <Name/>
            <SellersItemIdentification>
              <ID>SE0103</ID>
              <ExtendedID/>
            </SellersItemIdentification>
            <BuyersItemIdentification>
              <ID/>
            </BuyersItemIdentification>
          </Item>
          <SesExtension/>
        </LineItem>
        <SesExtension/>
      </OrderLine>
      <SesExtension>
        <ShippingMethodCode>standardMail</ShippingMethodCode>
      </SesExtension>
    </Order>
    ```

## Response from the ERP/Web-Connector

The ERP will provide a Price Response using the [UBL Format](../erp_communication/guides/usage_of_ubl.md). The whole ERP response object is returned back to the RemotePriceProvider and is used for the price calculation.

For each requested product - an orderLine in the response might be provided. If ERP doesn´t know the requested product, it might not return any information about this product at all. It is also possible that ERP will return more orderLines than requested.

This might have several reasons:

- additional costs such as shipping costs, costs for pay on delivery and others
- discounts
- additional products, that are MUST HAVE, if some special products were requested, or e.g. if the price totals of ordered products is over a specific amount

#### VAT Percent

If the ERP System does not provide the information about VAT percent, the VAT percent can be determined in the shop. In that case the shop uses the [VatService](../../../developer_manual/price_engine/price_engine_api/price_engine_services/vatserviceinterface/localvatservice.md) to get the vatPercent by the vatCode.

### SesExtension Fields

|SesExtension Field|Type|
|--- |--- |
|LineType|int|
|CostType|string|
|StockNumeric|int|
|OnStock|bool|
|VatCode|string|
|VatPercent|float|
|PriceIsIncVat|bool|

##### LineType

The tag LineType controls whether the returned orderLine is a product or a cost. 

|LineType|Description|
|--- |--- |
|1|cost|
|2|product|

##### CostType

The tag CostType indicates, which kind of cost is returned. 

|CostType|Description|
|--- |--- |
|shipping|indicates shipping costs|
|charge|indicates other (not shipping) costs|
|discount|indicates discounts|

##### StockNumeric

The tag StockNumeric indicates, how many items are on stock.

##### OnStock

The tag OnStock indicates, if the item is available on stock.

##### VatCode

The tag VatCode indicates the code for vat percent, e.g. 'download' or 'food'.

##### VatPercent

The tag VatPercent indicates the vat in %.

##### PriceIsIncVat

The tag PriceIsIncVat indicates, if the returned price is incl. vat or not.

!!! note

    Please notice, that either VatCode or VatPercent must be returned from ERP\!

??? note "Example OrderLine"

    ``` xml
    <OrderLine>
        <LineItem>
          <ID>123456789</ID>
          <SalesOrderID/>
          <UUID/>
          <Note/>
          <LineStatusCode/>
          <Quantity>1</Quantity>
          <LineExtensionAmount/>
          <MinimumQuantity/>
          <MaximumQuantity/>
          <MinimumBackorderQuantity/>
          <MaximumBackorderQuantity/>
          <InspectionMethodCode/>
          <PartialDeliveryIndicator/>
          <BackOrderAllowedIndicator/>
          <AccountingCostCode/>
          <AccountingCost/>
          <Delivery>
            <MaximumQuantity/>
            <LatestDeliveryDate/>
          </Delivery>
          <Price>
            <PriceAmount>7.90</PriceAmount>
          </Price>
          <TotalTaxAmount/>
          <Item singleElementArrays="Description">
            <SellersItemIdentification>
              <ID>SE0103</ID>
              <ExtendedID/>
            </SellersItemIdentification>
            <Name>DHL Standard</Name>
            <Description>Shipping costs DHL Standard</Description>
          </Item>
          <SesExtension>
            <LineType>1</LineType>
            <CostType>shipping</CostType>
            <StockNumeric/>
            <OnStock/>
            <VatCode/>
            <VatPercent>19</VatPercent>
            <PriceIsIncVat>1</PriceIsIncVat>
          </SesExtension>
        </LineItem>
      </OrderLine>
    ```
