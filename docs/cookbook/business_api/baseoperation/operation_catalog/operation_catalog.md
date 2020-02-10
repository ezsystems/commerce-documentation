#  Operation Catalog

This class implements business logic for basket

#### **Methods**

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
method
</th>
<th><div class="tablesorter-header-inner">
parameters
</th>
<th><div class="tablesorter-header-inner">
returns
</th>
<th><div class="tablesorter-header-inner">
purpose
</th>
<th><div class="tablesorter-header-inner">
operation identifier
</th>
</tr>
</thead>
<tbody>
<tr>
<td><a href="loadProducts_23560344.html">loadProducts</a></td>
<td>InputLoadList $input</td>
<td>OutputLoadList $input</td>
<td>load products from catalog</td>
<td>catalog.load_products</td>
</tr>
</tbody>
</table>

#### Service definition

**services.business\_layer.xml**

``` 
   <parameters>
        <parameter key="ses_eshop.catalog.business_api.class">Silversolutions\Bundle\EshopBundle\Services\BusinessLayer\Operations\Catalog</parameter>
    </parameters>

    <!-- catalog business component -->
    <service id="ses_eshop.catalog.business_api" class="%ses_eshop.catalog.business_api.class%"
                 parent="ses_eshop.business_api.base">
            <argument type="service" id="silver_catalog.catalog_service" />
            <tag name="business_api_operation" alias="catalog" />
    </service>
```
