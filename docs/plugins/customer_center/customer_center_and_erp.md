# Customer Center and ERP

The customer Center is connected to the ERP system in order to synchronize customer and contact data as good as possible/required:

- The ERP controls, which customer will get a customercenter inside the shop 
- The ERP already might know contacts employed in this company. These contacts will be shown in the customercenter as well

## Which customers will get an customercenter in the shop?

The ERP provides information back if a customer having a given customer number shall have a customer center or not.

The data is returned during the SelectCustomer request. 

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
|CustomerCenter|1: customer will get a customer center in the shop
0: no customer center|
|CustomerCenterMainContactEmail|This person having this email can activate the customer center only.</br>
He will become the MainContact Role|


## List of contacts from the ERP

The customer center displays a list of contacts which are not having an shop account right now.

The ERP provides a list back using the ERP method "SelectContact".  <span style="background-color: transparent;line-height: 1.4285715;">For each contact stored in the ERP one user record will be provided.  Special contacts (such as Company contacts) will be skipped. 

|Attribute|Description|
|--- |--- |
|ID|Unique Contact number from the ERP|
|Name|Name of the person|
|Telephone|Phone number|
|Telefax||
|ElectronicMail|email address of this person|
|LanguageCode|Code of the language|
