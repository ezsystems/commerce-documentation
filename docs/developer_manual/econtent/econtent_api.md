#  econtent - API 

econtent provides a felxible way to define project specific attributes for products and catagories (and other product related types of data).

eZ Commerce requires thats some of the attributes have to use predefined identifiers.

## Important econtent identifiers

### Categories

<table>
<thead>
<tr class="header">
<th>Identifier</th>
<th>type</th>
<th>description</th>
<th>standard mapping</th>
<th>Sroed in</th>
<th>Example data</th>
</tr>
</thead>
<tbody>
<tr>
<td>ses_name</td>
<td>ezstring</td>
<td>The name of the category</td>
<td>catalogElement.name</td>
<td>data_text</td>
<td>Kitchenware</td>
</tr>
<tr>
<td>code</td>
<td>ezstring</td>
<td>A unique id for the category</td>
<td><br />
</td>
<td><br />
</td>
<td>01-0998</td>
</tr>
<tr>
<td>text</td>
<td>ezstring</td>
<td>Longtext for the category</td>
<td>catalogElement.text</td>
<td>data_text</td>
<td>Our famous kitchenware ...</td>
</tr>
</tbody>
</table>

### Products

<table style="width:100%;">
<colgroup>
<col style="width: 19%" />
<col style="width: 7%" />
<col style="width: 19%" />
<col style="width: 41%" />
<col style="width: 0%" />
<col style="width: 12%" />
</colgroup>
<thead>
<tr class="header">
<th>Identifier</th>
<th>type</th>
<th>description</th>
<th>standard mapping</th>
<th>Stored in</th>
<th>Example data</th>
</tr>
</thead>
<tbody>
<tr>
<td>ses_name</td>
<td>ezstring</td>
<td>The name of the product</td>
<td>catalogElement.name</td>
<td>data_text</td>
<td>Blender</td>
</tr>
<tr>
<td>ses_sku</td>
<td>ezstring</td>
<td>The unique SKU</td>
<td>catalogElement.sku</td>
<td>data_text</td>
<td>HR2095/92</td>
</tr>
<tr>
<td>ses_subtitle</td>
<td>ezstring</td>
<td>The subtitle used in product listing pages</td>
<td><div class="content-wrapper">
<p>catalogElement.text and catalogElement.subtitle</p>

<p>catalogElement.text will be replaced by the new attribute subtitle in the future</p>
</td>
<td>data_text</td>
<td>Philips Avance Collection HR2095/92 blender 2 L Tabletop blender Black 700 W</td>
</tr>
<tr>
<td>ses_short_description</td>
<td>ezstring</td>
<td>A short description</td>
<td>catalogElement.shortDescription</td>
<td>data_text</td>
<td>700 W, 2 L</td>
</tr>
<tr>
<td>ses_long_description</td>
<td>ezstring</td>
<td><br />
</td>
<td>catalogElement.longDescription</td>
<td>data_text</td>
<td>&lt;b&gt;Make perfect smoothies and crush ice instantly&lt;/b&gt;&lt;br&gt;- Unique off-center jar design to mix ingredients efficiently&lt;br&gt;- Powerful 700 W motor&lt;br&gt;- Max 2 L (with food 1.5 L) high quality glass jar&lt;br&gt;- ProBlend 6 star blade for blending and cutting effectively&lt;br&gt;&lt;b&gt;Choose the exact speed you need&lt;/b&gt;&lt;br&gt;- Variable speed control&lt;br&gt;- One touch pulse and smoothie button&lt;br&gt;&lt;b&gt;Ease of use&lt;/b&gt;&lt;br&gt;- Spatula for mixing ingredients easily&lt;br&gt;&lt;b&gt;Easy to use and clean&lt;/b&gt;&lt;br&gt;- Easy cleaning with detachable blade&lt;br&gt;- All parts are dishwasher safe, except for the main unit</td>
</tr>
<tr>
<td>ses_unit_price</td>
<td>ezprice</td>
<td>The list price of the product based on the default currency</td>
<td>catalogElement.price</td>
<td>data_float</td>
<td>20,99</td>
</tr>
<tr>
<td>ses_vat_code</td>
<td>ezstring</td>
<td><br />
</td>
<td>catalogElement.vatCode</td>
<td>data_text</td>
<td><br />
</td>
</tr>
<tr>
<td>ses_unit</td>
<td>ezstring</td>
<td>The unit of the product</td>
<td>catalogElement.unit</td>
<td>data_text</td>
<td>PCS</td>
</tr>
<tr>
<td>ses_image_main</td>
<td>ezstring</td>
<td>relative path to the main image.</td>
<td>catalogElement.mainImage</td>
<td>data_text</td>
<td>img_19294139_high_1482454050_5917_19428.jpg</td>
</tr>
<tr>
<td>ses_specification</td>
<td>ezstring</td>
<td>A json formated set of data containing a flexible and grouped specifications</td>
<td>catalogElement.specifications</td>
<td>data_text</td>
<td>{"technic":[{"label":"Bowl capacity","value":"2 L"},{"label":"Pulse function","value":"Y"},{"label":"Type","value":"Tabletop blender"},{"label":"Removable bowl","value":"Y"},{"label":"Product colour","value":"Black"},{"label":"Cord storage","value":"Y"},{"label":"Cord length","value":"1 m"},{"label":"Easy to clean","value":"Y"},{"label":"Blending bowl material","value":"Glass"},{"label":"Housing material","value":"Polypropylene"},{"label":"Blade material","value":"Stainless steel"},{"label":"Power","value":"700 W"},{"label":"AC input voltage","value":"220 - 240 V"},{"label":"AC input frequency","value":"50 - 60 Hz"},{"label":"Power consumption (typical)","value":"700 W"}]}</td>
</tr>
<tr>
<td><br />
</td>
<td><br />
</td>
<td>A json formated set of data containig the variants for the product</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

**Data for variant products**

| Identifier                 | type                  | description                                                                                                                   | standard mapping                                          | Stored in  | Example data                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| -------------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ses\_variants | ezstring | Contains the sku/variantcode, name, list price and characteristic for the variants | will be converted in order to build an variantProductNode | data\_text | \[{"sku":{"label":"Sku","value":"0002"},"variantCode":{"label":"Variant Code","value":"D4142"},"description":{"label":"Description","value":"Bubble light bulb small"},"characteristicCode1":{"label":"small","value":"small"},"characteristicLabel1":{"label":"Color","value":"Color"},"characteristicCode2":{"label":"Silver","value":"Silver"},"characteristicLabel2":{"label":"","value":""},"priceNet":{"label":"Listprice net","value":"220"},"vatPercent":{"label":"VAT percent","value":""},"dataMap\_countryOfOrigin":{"label":"dataMap Country of Origin","value":""}},{"sku":{"label":"Sku","value":"0002"},"variantCode":{"label":"Variant Code","value":"D4143"},"description":{"label":"Description","value":"Bubble light bulb large"},"characteristicCode1":{"label":"large","value":"large"},"characteristicLabel1":{"label":"Size","value":"Size"},"characteristicCode2":{"label":"Silver","value":"Silver"},"characteristicLabel2":{"label":"Color","value":"Color"},"priceNet":{"label":"Listprice net","value":"220"},"vatPercent":{"label":"VAT percent","value":""},"dataMap\_countryOfOrigin":{"label":"dataMap Country of Origin","value":""}},{"sku":{"label":"Sku","value":"0002"},"variantCode":{"label":"Variant Code","value":"5532"},"description":{"label":"Description","value":"Alibaba Single Door Silver Refrigerator"},"characteristicCode1":{"label":30,"value":30},"characteristicLabel1":{"label":"Size","value":"Size"},"characteristicCode2":{"label":"White","value":"White"},"characteristicLabel2":{"label":"Color","value":"Color"},"priceNet":{"label":"Listprice net","value":"330"},"vatPercent":{"label":"VAT percent","value":""},"dataMap\_countryOfOrigin":{"label":"dataMap Country of Origin","value":""}}\] |

**Optional attributes**

<table>
<thead>
<tr class="header">
<th>Identifier</th>
<th>type</th>
<th>description</th>
<th>standard mapping</th>
<th>Stored in</th>
<th>Example data</th>
</tr>
</thead>
<tbody>
<tr>
<td>ses_country_of_origin</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>data_text</td>
<td><br />
</td>
</tr>
<tr>
<td>ses_ean</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>data_text</td>
<td>08710103626732</td>
</tr>
<tr>
<td>ses_brand</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>data_text</td>
<td>Phillips</td>
</tr>
<tr>
<td>ses_manufacturer</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>data_text</td>
<td>Phillips</td>
</tr>
<tr>
<td>ses_manfacturer_sku</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>data_text</td>
<td><br />
</td>
</tr>
<tr>
<td>ses_video</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>data_text</td>
<td>e.g. link to YouTube video</td>
</tr>
<tr>
<td>ses_image_list</td>
<td><br />
</td>
<td>Additional images</td>
<td>catalogElement.imageList</td>
<td>data_text</td>
<td><br />
</td>
</tr>
<tr>
<td>ses_max_order_quantity</td>
<td>ezinteger</td>
<td>The max quantity to be ordered per order</td>
<td>catalogElement.maxOrderQuantity</td>
<td>data_int</td>
<td>1000</td>
</tr>
<tr>
<td>ses_min_order_quantity</td>
<td>ezinteger</td>
<td><br />
</td>
<td>catalogElement.minOrderQuantity</td>
<td>data_int</td>
<td>1</td>
</tr>
<tr>
<td>ses_discontinued</td>
<td>ezboolean</td>
<td>If a product is discontinued it can be ordered only if stock &gt; 0</td>
<td>Stored in the dataMap</td>
<td>data_int</td>
<td>0 or 1</td>
</tr>
</tbody>
</table>
