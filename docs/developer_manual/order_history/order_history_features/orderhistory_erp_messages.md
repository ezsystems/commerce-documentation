# ERP messages

The module uses the following ERP messages:

The specifications are stored in  vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml.

The are defining the input and output structure for each message. 

|silver_erp.config.messages|Prozesstyp NAV|webservice_operation|status|
|--- |--- |--- |--- |
|invoice_detail|READPOSTEDSALESINVOICE|SV_OPENTRANS_GET_ORDERSTATUS|Resources/specifications/xml/request.invoiceDetail.xml</br>Resources/specifications/xml/response.invoiceDetail.xml|
|invoice_list|READPOSTEDSALESINVOICELIST|SV_OPENTRANS_GET_ORDERLIST|Resources/specifications/xml/request.invoiceList.xml</br>Resources/specifications/xml/response.invoiceList.xml|
|delivery_note_detail|READPOSTEDSALESSHIPMENT|SV_OPENTRANS_GET_ORDERSTATUS|Resources/specifications/xml/request.deliveryNoteDetail.xml</br>Resources/specifications/xml/response.deliveryNoteDetail.xml|
|delivery_note_list|READPOSTEDSALESSHIPMENTLIST|SV_OPENTRANS_GET_ORDERLIST|Resources/specifications/xml/request.deliveryNoteList.xml</br>Resources/specifications/xml/response.deliveryNoteList.xml|
|credit_memo_detail|READPOSTEDSALESCRMEMO|SV_OPENTRANS_GET_ORDERSTATUS|Resources/specifications/xml/request.creditMemoDetail.xml</br>Resources/specifications/xml/response.creditMemoDetail.xml|
|credit_memo_list|READPOSTEDSALESCRMEMOLIST|SV_OPENTRANS_GET_ORDERLIST|Resources/specifications/xml/request.creditMemoList.xml</br>Resources/specifications/xml/response.creditMemoList.xml|
|order_detail|READSALESDOCUMENT|SV_OPENTRANS_GET_ORDERSTATUS|Resources/specifications/xml/request.orderDetail.xml</br>Resources/specifications/xml/response.orderDetail.xml|
|order_list|READSALESDOCUMENTLIST|SV_OPENTRANS_GET_ORDERLIST|Resources/specifications/xml/request.orderList.xml</br>Resources/specifications/xml/response.orderList.xml|

## Example for getting an invoice

This example fetches a invoice number from the ERP system

``` php
$message = $this->container->get('silver_erp.message_inquiry_service')->inquireMessage(
    InvoiceDetailFactoryListener::INVOICEDETAIL
);
/** @var InvoiceDetailRequest $request */
$request = $message->getRequestDocument();
$request->ID->value = $invoiceNumber;
$request->CustomerNumber->value = $customerNumber;
/** @var WebConnectorMessageTransport $transport */
$transport = $this->container->get('silver_erp.message_transport');
$response = $transport->sendMessage($message)->getResponseDocument();
```

The specification for the request (request.invoiceDetail.xml) reflects the fields used in the request:

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceDetailRequest>
    <ID>10000</ID>
    <CustomerNumber>10000</CustomerNumber>
</InvoiceDetailRequest>
```

The response list defined in response.invoiceDetail.xml:

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<Invoice ses_unbounded="PaymentMeans PaymentTerms TaxTotal InvoiceLine" ses_tree="SesExtension">
    <ID></ID>
    <IssueDate></IssueDate>
    <InvoiceTypeCode>SalesInvoice</InvoiceTypeCode>
    <SesExtension>
        <Status></Status>
        <ShipmentMethod></ShipmentMethod>
        <ShippingAgentCode></ShippingAgentCode>
        <DiscountAmount></DiscountAmount>
    </SesExtension>
    <OrderReference>
        <ID></ID>
        <SalesOrderID></SalesOrderID>
        <IssueDate></IssueDate>
    </OrderReference>
    <AccountingSupplierParty>
    </AccountingSupplierParty>
    <AccountingCustomerParty ses_type="ses:Party">
        <Party></Party>
    </AccountingCustomerParty>
    <BuyerCustomerParty ses_type="ses:Party">
        <Party></Party>
    </BuyerCustomerParty>
    <Delivery ses_type="ses:DeliveryParty">
        <DeliveryParty></DeliveryParty>
    </Delivery>
    <PaymentMeans>
        <PaymentMeansCode>31</PaymentMeansCode>           <!-- Code for ERP (e.g. CREDIT, CASHONDELIV, ...) -->
        <PaymentDueDate>2007-01-01</PaymentDueDate>
        <PaymentChannelCode>IBAN</PaymentChannelCode>     <!-- PAYPAL, BANK, other payment providers ... -->
        <InstructionID>A12345</InstructionID>             <!-- Transaction ID for epayments -->
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
            <PrimaryAccountNumberID/>  <!-- e.g crypted/masked card number -->
            <NetworkID/> <!-- Master, Visa, .. -->
            <!--  <CardTypeCode>Debit, Credit etc. </CardTypeCode> -->
            <ExpiryDate/>
        </CardAccount>
    </PaymentMeans>
    <PaymentTerms>
        <Note></Note>
    </PaymentTerms>
    <TaxTotal>
        <TaxAmount></TaxAmount>
    </TaxTotal>
    <LegalMonetaryTotal>
        <TaxExclusiveAmount></TaxExclusiveAmount>
        <TaxInclusiveAmount></TaxInclusiveAmount>
        <PayableAmount></PayableAmount>
    </LegalMonetaryTotal>
    <InvoiceLine ses_tree="SesExtension">
        <ID></ID>
        <InvoicedQuantity></InvoicedQuantity>
        <Item>
            <Description></Description>
            <Name></Name>
            <SellersItemIdentification>
                <ID></ID>
                <ExtendedID>123</ExtendedID>
            </SellersItemIdentification>
        </Item>
        <Price>
            <PriceAmount></PriceAmount>
        </Price>
        <Delivery>
            <ActualDeliveryDate></ActualDeliveryDate>
        </Delivery>
        <SesExtension>
            <LineType></LineType>
            <BaseUnit></BaseUnit>
            <UnitPrice></UnitPrice>
            <TaxInclusiveAmount></TaxInclusiveAmount>
        </SesExtension>
    </InvoiceLine>
</Invoice>
```
