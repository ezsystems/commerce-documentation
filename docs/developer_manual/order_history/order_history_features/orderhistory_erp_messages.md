# Orderhistory - ERP messages 

The module uses the following ERP messages:

The specifications are stored in  vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/specifications/xml.

The are defining the input and output structure for each message. 

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
silver_erp.config.messages
</th>
<th><div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
Prozesstyp NAV
</th>
<th><div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
webservice_operation
</th>
<th><div class="tablesorter-header-inner">
<div class="tablesorter-header-inner">
status

</th>
</tr>
</thead>
<tbody>
<tr>
<td>invoice_detail</td>
<td>READPOSTEDSALESINVOICE</td>
<td>SV_OPENTRANS_GET_ORDERSTATUS</td>
<td><p>Resources/specifications/xml/request.invoiceDetail.xml</p>
<p>Resources/specifications/xml/response.invoiceDetail.xml</p></td>
</tr>
<tr>
<td>invoice_list</td>
<td>READPOSTEDSALESINVOICELIST</td>
<td>SV_OPENTRANS_GET_ORDERLIST</td>
<td><p>Resources/specifications/xml/request.invoiceList.xml</p>
<p>Resources/specifications/xml/response.invoiceList.xml</p></td>
</tr>
<tr>
<td>delivery_note_detail</td>
<td>READPOSTEDSALESSHIPMENT</td>
<td>SV_OPENTRANS_GET_ORDERSTATUS</td>
<td><p>Resources/specifications/xml/request.deliveryNoteDetail.xml</p>
<p>Resources/specifications/xml/response.deliveryNoteDetail.xml</p></td>
</tr>
<tr>
<td>delivery_note_list</td>
<td>READPOSTEDSALESSHIPMENTLIST</td>
<td>SV_OPENTRANS_GET_ORDERLIST</td>
<td><p>Resources/specifications/xml/request.deliveryNoteList.xml</p>
<p>Resources/specifications/xml/response.deliveryNoteList.xml</p></td>
</tr>
<tr>
<td>credit_memo_detail</td>
<td>READPOSTEDSALESCRMEMO</td>
<td>SV_OPENTRANS_GET_ORDERSTATUS</td>
<td><p>Resources/specifications/xml/request.creditMemoDetail.xml</p>
<p>Resources/specifications/xml/response.creditMemoDetail.xml</p></td>
</tr>
<tr>
<td>credit_memo_list</td>
<td>READPOSTEDSALESCRMEMOLIST</td>
<td>SV_OPENTRANS_GET_ORDERLIST</td>
<td><p>Resources/specifications/xml/request.creditMemoList.xml</p>
<p>Resources/specifications/xml/response.creditMemoList.xml</p></td>
</tr>
<tr>
<td>order_detail</td>
<td>READSALESDOCUMENT</td>
<td>SV_OPENTRANS_GET_ORDERSTATUS</td>
<td><p>Resources/specifications/xml/request.orderDetail.xml</p>
<p>Resources/specifications/xml/response.orderDetail.xml</p></td>
</tr>
<tr>
<td>order_list</td>
<td>READSALESDOCUMENTLIST</td>
<td>SV_OPENTRANS_GET_ORDERLIST</td>
<td><p>Resources/specifications/xml/request.orderList.xml</p>
<p>Resources/specifications/xml/response.orderList.xml</p></td>
</tr>
</tbody>
</table>

## Example for getting an invoice

This example fetches a invoice number from the ERP system

``` 
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

``` 
<?xml version="1.0" encoding="UTF-8"?>
<InvoiceDetailRequest>
    <ID>10000</ID>
    <CustomerNumber>10000</CustomerNumber>
</InvoiceDetailRequest>
```

The reponse ist defined in response.invoiceDetail.xml:

``` 
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
