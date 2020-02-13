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

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Description</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>CustomerCenter</code></pre></td>
<td><p>1: customer will get a customer center in the shop</p>
<p>0: no customer center</p></td>
<td><br />
</td>
</tr>
<tr class="even">
<td><pre><code>CustomerCenterMainContactEmail</code></pre></td>
<td><p>This person having this email can activate the customer center only.</p>
<p>He will become the MainContact Role</p></td>
<td><br />
</td>
</tr>
</tbody>
</table>

## List of contacts from the ERP

The customer center displays a list of contacts which are not having an shop account right now.

The ERP provides a list back using the ERP method "SelectContact".  <span style="background-color: transparent;line-height: 1.4285715;">For each contact stored in the ERP one user record will be provided.  Special contacts (such as Company contacts) will be skipped. 

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>ID</td>
<td>Unique Contact number from the ERP</td>
</tr>
<tr class="even">
<td>Name</td>
<td>Name of the person</td>
</tr>
<tr class="odd">
<td>Telephone</td>
<td><p>Phone number</p>
<p><br />
</p></td>
</tr>
<tr class="even">
<td>Telefax</td>
<td><br />
</td>
</tr>
<tr class="odd">
<td>ElectronicMail</td>
<td>email address of this person</td>
</tr>
<tr class="even">
<td>LanguageCode</td>
<td>Code of the language</td>
</tr>
</tbody>
</table>
