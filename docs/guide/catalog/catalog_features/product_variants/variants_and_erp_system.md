# Variants and ERP system

It is important to differentiate between the variants used in eZ Commerce and how variants are stored in the ERP system.
eZ Commerce is able to handle two different approaches to variants. This reflects how they are set in the ERP system.

## Storage in ERP

There are two ways in which ERP systems handle variants:

- The ERP system can use an SKU and offer a list of variant codes for each individual products (e.g. a shoe in size 43 and in green). In this case the product is identified by the SKU and the `variantCode`: `SKU_AND_VARIANT`
- Often the ERP system doesn't store variants as "real" variants but uses a unique SKU for each variant. Often these "variants" are grouped by a PIM system or a special field in the ERP system. In this case, the `variantCode` from the catalog is the actual SKU that is sent to the ERP: `SKU_ONLY`

eZ Commerce supports both methods. To select a method, use the following configuration:

``` yaml
parameters: 
    # possible values: SKU_ONLY (default), SKU_AND_VARIANT
    silver_eshop.default.erp.variant_handling: "SKU_ONLY"
```

## Price requests

When a price is requested from the ERP, the shop sends the SKU and optionally a variant code as well.

If `SKU_AND_VARIANT` is used then the UBL request contains the SKU (ID) and the variant code (ExtendedID):

``` xml
<Item>
    <Name/>
    <SellersItemIdentification>
        <ID>1000</ID>
        <ExtendedID>VAR-GRN</ExtendedID>
    </SellersItemIdentification>
    <BuyersItemIdentification>
        <ID/>
    </BuyersItemIdentification>
</Item>
```

If `SKU_ONLY` is used, the shop sends the variant code in both fields to the ERP. The variant code contains the SKU in this case. 
