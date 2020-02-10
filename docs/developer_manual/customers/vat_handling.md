#  VAT handling 

The VAT handling for customers inside eZ Commerce is controlled by:

  - The ERP (Advanced version only) system
  - The settings from the user object 
  - Default settings configured in a yaml file

The ERP system will have the highest priority. It means if it can provide the info about the VAT handling this setting will win. 

If the attributes are setup in the user object these settings will be used in case the ERP is not used (eZ Commerce) or the ERP does not provide this info.

As a last fall back the settings defined per siteaccess are used. 

## General infos about the settings

<table>
<thead>
<tr class="header">
<th><br />
</th>
<th>Use case</th>
<th>What happens in the shop</th>
</tr>
</thead>
<tbody>
<tr>
<td>Customer has to pay VAT</td>
<td><p>set to false:</p>
<p>If for legal reasons the customer do not have to pay VAT (this is usually the case for some B2B cases).</p></td>
<td>If set to false the shop will not calculate VAT</td>
</tr>
<tr>
<td><p>Display Price inc VAT</p></td>
<td><p>set to false:</p>
<p>This should be set to false only if a customer does not have to pay VAT</p></td>
<td>If set to false the price in the shop will be without VAT </td>
</tr>
</tbody>
</table>

## VAT settings from the ERP (Advanced version only)

The ERP can provide VAT settings per customer when a selectcustomer request is sent after a login.

There are 2 attributes which can be set by the ERP system:

  - SesExtension-\>HasToPayVat 
  - SesExtension-\>DisplayPriceInclVat

The default mapping is defined in the mapping xslt file. The default mapping file is located in silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/mapping/wc3-nav/xsl/response.select\_customer.xsl

``` 
<CustomerResponse>
  <SesExtension>
    <!-- Has to be adjusted depending on ERP -->
    <xsl:choose>
        <xsl:when test="VAT_Bus_Posting_Group = 'NATIONAL'">
            <HasToPayVat>1</HasToPayVat>
        </xsl:when>
        <xsl:otherwise>
            <HasToPayVat>0</HasToPayVat>
            <DisplayPriceInclVat>0</DisplayPriceInclVat>
        </xsl:otherwise>
    </xsl:choose>
  </SesExtension>
```

## VAT settings from the user object 

The VAT handling can be defined by user. 

![](attachments/23561055/23561256.png)

**Please note to use the correct identifiers  and to check the checkox "Default value"\!**

![](attachments/23561055/23561254.png)

## VAT settings from the configuration

    ses.customer_profile_data.isPriceInclVat: true
    ses.customer_profile_data.setHasToPayVat: true

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-12-12\_18-56-44.png](attachments/23561055/23561252.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-12-12\_19-1-29.png](attachments/23561055/23561254.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-12-12\_19-2-57.png](attachments/23561055/23561256.png) (image/png)  
