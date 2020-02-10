#  ERP Message: CalculateSalesOrder / CreateSalesOrder

Due to technical restrictions of the message generator, the messages CalculateSalesOrder and CreateSalesOrder are identical (except of the name). Changes in one specification XML file would the generator cause to change the PHP files of the other message, too. Thus, if one specification is changed, the other file MUST be changed, too.

CalculateSalesOrder fetches prices, which are calculated by the ERP system depending on the request data. Request data covers the customer number, the item numbers with quantities and maybe additional data like coupon codes or a campaign. In the standard implementation it will only send customer number and item data.

CreateSalesOrder submits an order to the ERP system. If the order could be processed successfully, the ERP will respond with a valid number in the SalesOrderID element.

The optional parameter "variantCode" in the line data will be mapped to the Ubl Field "ExtendedID".

## Request XML

``` 
<?xml version="1.0" encoding="UTF-8"?>
<Order ses_unbounded="OrderLine" ses_tree="SesExtension">
    <!-- WARNING: calculateSalesPrice needs to be the same structure as createSalesOrder! -->
    <DocumentCurrencyCode>EUR</DocumentCurrencyCode>
    <UUID></UUID>
    <IssueDate>2005-06-20</IssueDate>
    <Note>sample</Note>
    <SalesOrderID></SalesOrderID>
    <CustomerReference></CustomerReference>
    <BuyerCustomerParty>
        <SupplierAssignedAccountID>1234</SupplierAssignedAccountID>
        <Party ses_unbounded="PartyIdentification">
            <PartyIdentification>
                <ID>10000</ID>
            </PartyIdentification>
        </Party>
    </BuyerCustomerParty>
    <!-- will not be used but it can be used in project specific impl. -->
    <SellerSupplierParty ses_type="ses:Party">
        <Party>
        </Party>
    </SellerSupplierParty>
    <!-- will not be used but it can be used in project specific impl.
         AccountingCustomerParty will be the party getting the invoice
     -->
    <AccountingCustomerParty ses_type="ses:Party">
        <Party>
        </Party>
    </AccountingCustomerParty>
    <Delivery ses_type="ses:DeliveryParty">
        <RequestedDeliveryPeriod>
            <EndDate>2005-06-30</EndDate>
            <EndTime>18:00:00.0Z</EndTime>
        </RequestedDeliveryPeriod>
        <DeliveryParty />
    </Delivery>
    <PaymentMeans>
        <PaymentMeansCode>31</PaymentMeansCode>           <!-- Code for ERP (e.g. CREDIT, CASHONDELIV, ...) -->
        <PaymentDueDate>2007-01-01</PaymentDueDate>
        <PaymentChannelCode>IBAN</PaymentChannelCode>     <!-- PAYPAL, BANK, other payment providers ... -->
        <InstructionID>A12345</InstructionID>             <!-- Transaction ID for e-payments -->
        <!-- Just used for bank transfers / Lastschrift -->
        <PayeeFinancialAccount>
            <ID>IS000001261234560101901239</ID>           <!-- IBAN -->
            <CurrencyCode>EUR</CurrencyCode>
            <FinancialInstitutionBranch>
                <ID>SEISISRE</ID>                     <!-- BIC-->
                <Name>Central bank of Iceland</Name>  <!-- Name of Bank -->
            </FinancialInstitutionBranch>
        </PayeeFinancialAccount>
        <CardAccount>
            <PrimaryAccountNumberID></PrimaryAccountNumberID>  <!-- e.g crypted/masked card number -->
            <NetworkID></NetworkID> <!-- Master, Visa, .. -->
            <!--  <CardTypeCode>Debit, Credit etc. </CardTypeCode> -->
            <ExpiryDate></ExpiryDate>
        </CardAccount>
    </PaymentMeans>
    <TransactionConditions>
        <ID>3WEEKS</ID>
    </TransactionConditions>
    <OrderLine ses_tree="SesExtension" ses_type="ses:LineItem">
        <Note>Freetext note on line 1</Note>
        <Note>Freetext note on line 2</Note>
        <LineItem />
        <SesExtension />
    </OrderLine>
    <SesExtension>
    </SesExtension>
</Order>
```

Elements:

  - **UUID**
    
      - This is the GUID for the order document

  - **SalesOrderID**
    
      - This is reserved for the ERP's order ID. It is normaly not sent in the request.

  - **DocumentCurrencyCode**
    
      - This should define the desired currency.

  - **BuyerCustomerParty/Party/PartyIdentification/ID**
    
      - Although PartyIdentification's cardinality is unbounded, it is used only once and defines the requested customer number.

## Response XML

``` 
<?xml version="1.0" encoding="UTF-8"?>
<OrderResponse ses_unbounded="Delivery OrderLine">
    <!-- WARNING: calculateSalesPrice needs to be the same structure as createSalesOrder! -->
    <SalesOrderID></SalesOrderID>
    <DocumentCurrencyCode/>
    <IssueDate>2013-06-17</IssueDate>
    <BuyerCustomerParty ses_type="ses:Party">
        <Party>
        </Party>
    </BuyerCustomerParty>
    <AccountingCustomerParty ses_type="ses:Party">
        <Party>
        </Party>
    </AccountingCustomerParty>
    <Delivery ses_type="ses:DeliveryParty">
        <DeliveryParty />
    </Delivery>
    <OrderLine ses_type="ses:LineItem">
        <LineItem>
        </LineItem>
    </OrderLine>
    <TaxTotal>
        <TaxAmount>1262.50</TaxAmount> <!-- TAX -->
    </TaxTotal>
    <LegalMonetaryTotal>
        <TaxExclusiveAmount>1262.50</TaxExclusiveAmount> <!-- Amount excl. VAT -->
        <TaxInclusiveAmount>6312.50</TaxInclusiveAmount> <!-- Amount inc. VAT -->
        <PayableAmount>6312.50</PayableAmount> <!-- Amount incl. VAT and all allowances and charges-->
    </LegalMonetaryTotal>
</OrderResponse>
```

Elements:

  - The parties should be the same as in [SelectCustomer](23560378.html)

  - **IssueDate**
    
      - Is only be used in CreateSalesOrder message

  - **DocumentCurrencyCode**
    
      - This should define the desired currency.

  - **Delivery/DeliveryAddress**
    
      - Should be the same as in [SelectCustomer](23560378.html)

  - **LegalMonetaryTotal**
    
      - Contains information about summarized prices
      - **PayableAmount**
          - Is a must have field in UBL standard

### Reusable Party element

Please see [Reusable Party element](23560378.html#ERPMessage:SelectCustomer-ReusablePartyelement)

### Reusable LineItem element

``` 
<LineItem ses_unbounded="Delivery AllowanceCharge" ses_tree="SesExtension">
    <ID/>
    <SalesOrderID>10000</SalesOrderID>
    <Quantity>1</Quantity>
    <LineExtensionAmount/>
    <TotalTaxAmount/>
    <MinimumQuantity/>
    <MaximumQuantity/>
    <MinimumBackorderQuantity/>
    <MaximumBackorderQuantity/>
    <PartialDeliveryIndicator/>
    <BackOrderAllowedIndicator/>
    <Delivery>
        <MaximumQuantity>0</MaximumQuantity>
        <LatestDeliveryDate/>
    </Delivery>
    <AllowanceCharge>
        <ID/>
        <ChargeIndicator>true|false</ChargeIndicator>
        <AllowanceChargeReasonCode/>
        <AllowanceChargeReason/>
        <Amount/>
    </AllowanceCharge>
    <Price>
        <PriceAmount>2950</PriceAmount>
        <BaseQuantity>1</BaseQuantity>
    </Price>
    <Item ses_unbounded="Description ManufacturersItemIdentification">
        <Name>Web-Connector</Name>
        <Description>für SAP, Magento oder NAV</Description>
        <SellersItemIdentification>
            <ID>1000</ID>
            <ExtendedID>123</ExtendedID>
        </SellersItemIdentification>
        <ManufacturersItemIdentification>
            <ID>ABC</ID>
        </ManufacturersItemIdentification>
        <BuyersItemIdentification>
            <ID>ABC</ID>
        </BuyersItemIdentification>
    </Item>
    <SesExtension>
        <StockNumeric/>
        <OnStock/>
        <VatCode/>
        <PriceIsIncVat/>
    </SesExtension>
</LineItem>
```

Elements:

  - **ID**
    
      - The line identification, assigned by the customer. It will be rarely used. Possible usage is if the shop system assigns a line ID which differs from the ERP system. The field is mandatory, even if it may be empty.

  - **SalesOrderID**
    
      - The seller identification for the line record. May be set by the ERP system.

  - **Quantity**
    
      - The requested number of items.

  - **Delivery/MaximumQuantity**
    
      - Used to store the stock info

  - **Delivery/LatestDeliveryDate**
    
      - Used to store the availability. Must be an ISO date string.

  - **Price/PriceAmount**
    
      - Unit list price

  - **Item**
    
      - Item catalog information.
      - There are more item identification fields in this record (besides SellersItemIdentification). It should be discussed whether these are necessary (none of them are required).

  - **SesExtension**
    
      - OnStock: boolean 1=Yes, 0=no
    
      - StockNumeric: the number of units on stock
    
      - VatCode: string that identifies tax class
    
      - PriceIsIncVat: flag which indicates whether PriceAmount in inclusive taxes.  

      - May contain additional data from the ERP
