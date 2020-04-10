# Variants and ERP system

It is important to differentiate between the variants used in eZ Commerce and how variants are stored in the ERP system.  eZ Commerce is able to handle 2 different approaches to variants. This reflects how they are set in ERP system. 

!!! note "Storage in ERP"

    - 'Real variants': The ERP-systems uses a SKU and offers a list of variant codes for each individual products (e.g. a shoe in size 43 and in green). In this case the product is identified by the SKU and the variantCode. --\> SKU\_AND\_VARIANT
    - 'Not real variants': Often the ERP-system doesn't store variants as "real" variants but just use an unique SKU for each variant. Often these "variants" are grouped by a PIM system or a special field in the ERP system. In this case, the variantCode from the catalog will be the actual SKU, that is sent to the ERP. --\> SKU\_ONLY

    eZ Commerce supports both models, see Configuration for ERP handling regarding PriceRequest.

#### Configuration for ERP Handling regarding PriceRequest:

``` yaml
parameters: 
    # possible values: SKU_ONLY (default), SKU_AND_VARIANT
    silver_eshop.default.erp.variant_handling: "SKU_ONLY"
```

## Price requests

When a price is requested from the ERP the shop will send the SKU and maybe a variant code as well:

If `SKU_AND_VARIANT` is used then the UBL request will contain the SKU (ID) and the variant code (ExtendedID):

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

If `SKU_ONLY` is used the shop will send the variant-code in both fields towards the ERP. The variant code contains the SKU in this case. 
