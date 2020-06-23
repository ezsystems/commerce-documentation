# Customer center and ERP

The Customer center connects to the ERP system to synchronize customer and contact data:

The ERP controls which customer gets access to a Customer center inside the shop.
The ERP may already have information about contacts employed in the company. These contacts are shown in the Customer center as well.

The ERP provides information whether a customer with a given customer number should have access to the Customer center or not.

The data is returned during the `SelectCustomer` request. 

```
[BuyerCustomerParty] => Array
    (
    [Party]
        [SesExtension] => Array
            (
                [CustomerPostingGroup] => INLAND
                [CustomerCenter] => 1
                [CustomerCenterMainContactEmail] => tja@silversolutions.de
            )
```

|Attribute|Description|
|--- |--- |
|`CustomerCenter`|`1` - customer gets access to a Customer center in the shop; `0` - no Customer center|
|`CustomerCenterMainContactEmail`|Only the owner of this email can activate the Customer center. They become the main contact|

## List of contacts from the ERP

The customer center displays a list of contacts which do not have a shop account.

The ERP provides this list using the ERP method `SelectContact`. For each contact stored in the ERP,
one user record is provided. Special contacts (such as Company contacts) are skipped. 

|Attribute|Description|
|--- |--- |
|`ID`|Unique Contact number from the ERP|
|`Name`|Name of the person|
|`Telephone`|Phone number|
|`Telefax`||
|`ElectronicMail`|Email address|
|`LanguageCode`||
