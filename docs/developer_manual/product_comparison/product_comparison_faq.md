#  Product comparison - FAQ 

How to specify logic for categories

In order to change the logic of determining to which categories should be assigned to the particular products, there is a need to override or implement new comparison service.

First of all the service must implement "[ComparisonServiceInterface](Product-comparison---API_23560693.html)". There is a method, called getComparisonCategory(), which gets a CatalogElement (thus also products) passed as an argument. This method must implement the logic for the determination of a  comparison category for the passed product.

Our standard logic implementation:

1.  Try to take product type from CatalogElement
2.  Try to take it from parent element category
3.  If all fails put it to default category

Where are product attributes stored for comparison lists

Product attributes, that are used for comparison, come from CatalogElement. The standard implementation of the eZ Commerce stores CatalogElements in the eZ Platform CMS.

There are 2 types of information:

<table>
<thead>
<tr class="header">
<th>Type</th>
<th>Attribute Names</th>
<th>Features</th>
</tr>
</thead>
<tbody>
<tr>
<td>Dynamic fields from dataMap attribute</td>
<td><p>manufacturer</p>
<p>manufacturerSku</p>
<p>color</p></td>
<td><p>Those attributes are stored in dataMap. They are part of Technical Information group.</p>
<p>Manufacturer, ManufacturerSku and Color is handled without eZ matrix. The names that should be read from the dataMap must be configured in the parameter: siso_comparison.&lt;default&gt;.technical_attributes</p></td>
</tr>
<tr>
<td>Dynamic fields from specification attribute</td>
<td>specification</td>
<td><div class="content-wrapper">
<p>See <a href="SpecificationsType_23560733.html">SpecificationsType</a></p>
<p><br />
</p>
</td>
</tr>
</tbody>
</table>

How collapse of groups and attributes works

It can happen that in one comparison list you have many products and one of the attributes is the same for every product.

It means that you show a row in the table, which have the same value. eZ Commerce have different possibilities to handle that kind of situation.

#### Show/Hide attributes for the same value

There is an icon in top left corner to show/hide attributes. If set to true hiding/showing same attributes is enabled.

``` 
# each attribute that have the same values for each product can be hidden
siso_comparison.default.collapse_attributes: true
```

#### Collapsing all group for product attributes

There is a parameter that defines if the groups with all identical values should be collapsed or not (by default). 

This means that all rows inside a group are identical.

``` 
# behaviour for groups were all attributes have the same values
siso_comparison.default.collapse_groups: true
```

How sorting works with drag & drop 

We need to divide this into 2 lists: private (for customer) and public.

<table>
<thead>
<tr class="header">
<th>Public List</th>
<th>Private List</th>
</tr>
</thead>
<tbody>
<tr>
<td><p>For public lists user can use drag &amp; drop functionality, but it will not be saved.</p>
<p>Only administrator is able to change the order.</p>
<p>It means that visually it will be moved by not stored. When page is refreshed order will remain.</p></td>
<td><p>For private lists when drag and drop is done, ajax call is triggered with new sort order for the chosen list. </p>
<p>Sorting functionality is done by basket line's attribute "<strong>groupOrder</strong>". It is a part of BasketLine entity.</p></td>
</tr>
</tbody>
</table>

How to control which system shall provide prices and stock information

Using a configuration parameter it can be defined which system shall provide the prices and stock infos.

By default the ERP will be requested if it is not available the local price provider will respond. 

``` 
siso_price.default.price_service_chain.comparison:
        - siso_price.price_provider.remote
        - siso_price.price_provider.local
```
