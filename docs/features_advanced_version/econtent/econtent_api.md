# econtent - API

econtent provides a flexible way to define project specific attributes for products and categories (and other product related types of data).

eZ Commerce requires that some of the attributes have to use predefined identifiers.

## Important econtent identifiers

### Categories

|Identifier|type|description|standard mapping|Stored in|Example data|
|--- |--- |--- |--- |--- |--- |
|ses_name|ezstring|The name of the category|catalogElement.name|data_text|Kitchenware|
|code|ezstring|A unique id for the category|||01-0998|
|text|ezstring|Longtext for the category|catalogElement.text|data_text|Our famous kitchenware ...|

### Products

|Identifier|type|description|standard mapping|Stored in|Example data|
|--- |--- |--- |--- |--- |--- |
|ses_name|ezstring|The name of the product|catalogElement.name|data_text|Blender|
|ses_sku|ezstring|The unique SKU|catalogElement.sku|data_text|HR2095/92|
|ses_subtitle|ezstring|The subtitle used in product listing pages|catalogElement.text and catalogElement.subtitle</br>catalogElement.text will be replaced by the new attribute subtitle in the future|data_text|Philips Avance Collection HR2095/92 blender 2 L Tabletop blender Black 700 W|
|ses_short_description|ezstring|A short description|catalogElement.shortDescription|data_text|700 W, 2 L|
|ses_long_description|ezstring||catalogElement.longDescription|data_text|<b>Make perfect smoothies and crush ice instantly</b><br>- Unique off-center jar design to mix ingredients efficiently<br>- Powerful 700 W motor<br>- Max 2 L (with food 1.5 L) high quality glass jar<br>- ProBlend 6 star blade for blending and cutting effectively<br><b>Choose the exact speed you need</b><br>- Variable speed control<br>- One touch pulse and smoothie button<br><b>Ease of use</b><br>- Spatula for mixing ingredients easily<br><b>Easy to use and clean</b><br>- Easy cleaning with detachable blade<br>- All parts are dishwasher safe, except for the main unit|
|ses_unit_price|ezprice|The list price of the product based on the default currency|catalogElement.price|data_float|20,99|
|ses_vat_code|ezstring||catalogElement.vatCode|data_text||
|ses_unit|ezstring|The unit of the product|catalogElement.unit|data_text|PCS|
|ses_image_main|ezstring|relative path to the main image.|catalogElement.mainImage|data_text|img_19294139_high_1482454050_5917_19428.jpg|
|ses_specification|ezstring|A json formated set of data containing a flexible and grouped specifications|catalogElement.specifications|data_text|{"technic":[{"label":"Bowl capacity","value":"2 L"},{"label":"Pulse function","value":"Y"},{"label":"Type","value":"Tabletop blender"},{"label":"Removable bowl","value":"Y"},{"label":"Product colour","value":"Black"},{"label":"Cord storage","value":"Y"},{"label":"Cord length","value":"1 m"},{"label":"Easy to clean","value":"Y"},{"label":"Blending bowl material","value":"Glass"},{"label":"Housing material","value":"Polypropylene"},{"label":"Blade material","value":"Stainless steel"},{"label":"Power","value":"700 W"},{"label":"AC input voltage","value":"220 - 240 V"},{"label":"AC input frequency","value":"50 - 60 Hz"},{"label":"Power consumption (typical)","value":"700 W"}]}|
|||A json formated set of data containig the variants for the product||||

**Data for variant products**

| Identifier  | type |description     | standard mapping   | Stored in  | Example data    |
| -------- | -------- | ---------- | ------------------- | ---------- | ---- |
| ses\_variants | ezstring | Contains the sku/variantcode, name, list price and characteristic for the variants | will be converted in order to build an variantProductNode | data\_text | \[{"sku":{"label":"Sku","value":"0002"},"variantCode":{"label":"Variant Code","value":"D4142"},"description":{"label":"Description","value":"Bubble light bulb small"},"characteristicCode1":{"label":"small","value":"small"},"characteristicLabel1":{"label":"Color","value":"Color"},"characteristicCode2":{"label":"Silver","value":"Silver"},"characteristicLabel2":{"label":"","value":""},"priceNet":{"label":"Listprice net","value":"220"},"vatPercent":{"label":"VAT percent","value":""},"dataMap\_countryOfOrigin":{"label":"dataMap Country of Origin","value":""}},{"sku":{"label":"Sku","value":"0002"},"variantCode":{"label":"Variant Code","value":"D4143"},"description":{"label":"Description","value":"Bubble light bulb large"},"characteristicCode1":{"label":"large","value":"large"},"characteristicLabel1":{"label":"Size","value":"Size"},"characteristicCode2":{"label":"Silver","value":"Silver"},"characteristicLabel2":{"label":"Color","value":"Color"},"priceNet":{"label":"Listprice net","value":"220"},"vatPercent":{"label":"VAT percent","value":""},"dataMap\_countryOfOrigin":{"label":"dataMap Country of Origin","value":""}},{"sku":{"label":"Sku","value":"0002"},"variantCode":{"label":"Variant Code","value":"5532"},"description":{"label":"Description","value":"Alibaba Single Door Silver Refrigerator"},"characteristicCode1":{"label":30,"value":30},"characteristicLabel1":{"label":"Size","value":"Size"},"characteristicCode2":{"label":"White","value":"White"},"characteristicLabel2":{"label":"Color","value":"Color"},"priceNet":{"label":"Listprice net","value":"330"},"vatPercent":{"label":"VAT percent","value":""},"dataMap\_countryOfOrigin":{"label":"dataMap Country of Origin","value":""}}\] |

**Optional attributes**

|Identifier|type|description|standard mapping|Stored in|Example data|
|--- |--- |--- |--- |--- |--- |
|ses_country_of_origin||||data_text||
|ses_ean||||data_text|08710103626732|
|ses_brand||||data_text|Phillips|
|ses_manufacturer||||data_text|Phillips|
|ses_manfacturer_sku||||data_text||
|ses_video||||data_text|e.g. link to YouTube video|
|ses_image_list||Additional images|catalogElement.imageList|data_text||
|ses_max_order_quantity|ezinteger|The max quantity to be ordered per order|catalogElement.maxOrderQuantity|data_int|1000|
|ses_min_order_quantity|ezinteger||catalogElement.minOrderQuantity|data_int|1|
|ses_discontinued|ezboolean|If a product is discontinued it can be ordered only if stock > 0|Stored in the dataMap|data_int|0 or 1|
