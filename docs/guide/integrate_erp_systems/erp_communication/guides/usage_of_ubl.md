# Usage of UBL

## Commonly used fields

The eZ Commerce uses UBL (Universal Business Language) as a standard for the communication between the ERP systems and the shop. UBL is used also for representing data in the shop such as the customer data.

Since UBL offers different options e.g. to transport the customer number or other fields this document describes which fields were used by the eZ Commerce.

|Field|UBL field|Used in|
|--- |--- |--- |
|Customer number|Party/PartyIdentification/ID|Orders, Customerdata, price calculation|
|SKU|Item/SellersItemIdentification/ID|Orders, price calculation|

## Extensions to UBL

## Proposal 1, the UBL-way

At the beginning of very UBL there is a root element which can be used for extending the XML.

Pros:

- Keeping the XML conform to the UBL standard

Cons:

- Not very flexible due to the fact that the extension is only possible at the root of a UBL document
- This would break some of our messages (e. g.) where we **do not use** a complete UBL document as message format

## Proposal 2, an `<SesExtension>` in every `ses_type`-element

A pragmatic solution would be to create a dynamic entry point for extensions. For this every `ses_type`-element could contain and `<SesExtension>`-child element in which dynamic fields can be implemented.

Pros:

- We could extend at every point we would need to
- Implementation of the generator/converter could be extended to automatically fill/read these fields  
    e. g. into the attribute "dataMap" of an catalogElement

Cons:

- Completely 'own implementation'
- Documents will no longer validate to UBL standards

Example:

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<OrderResponse>
  <ErpContact ses_type="ses:Contact">
    <Contact>
      <ID>KT1001</ID>
      <Name>Mr Fred Churchill</Name>
      <Telephone>+44 127 2653214</Telephone>
      <Telefax>+44 127 2653215</Telefax>
      <ElectronicMail>fred@iytcorporation.gov.uk</ElectronicMail>
      <OtherCommunication></OtherCommunication>
      <Note></Note>
      <SesExtension>
        <LanguageCode></LanguageCode>
        <IsMain></IsMain>
      </SesExtension>
    </Contact>
  </ErpContact>
</OrderResponse>
```
