#  Configuration settings 

Here you can find a list of configurable settings for your shop.

In order to change the settings go to eZ Commerce in the top menu → eCommerce-\>Configuration settings.

For additional configuration settings use yml files to add the required settings.

![](attachments/23561024/23571013.png)

Configuration is divided into tabs. Each tab contains multiple sections that offer specific settings. 

**Important: Please note that settings defined in the backend will always override a configuration which is defined in Yml files\!**

## Catalog

The catalog section defines details for structure and layout of the catalog.

<table style="width:100%;">
<colgroup>
<col style="width: 17%" />
<col style="width: 33%" />
<col style="width: 6%" />
<col style="width: 42%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Layout product category page </td>
<td>
siso_core.default.category_view: product_list
<p><br />
</p>
</td>
<td><p>product_list</p></td>
<td><p>This setting changes the layout for the product category page.</p>
<p>category:</p>
<ul>
<li>show category boxes only</li>
</ul>
<p>product_list:</p>
<ul>
<li>show products only</li>
</ul>
<p>both:</p>
<ul>
<li>first categories are shown</li>
<li>afterwards products are displayed<br />
<br />
</li>
</ul>
<p>! Note ! The http-cache has to be emptied to make a change visible!</p></td>
</tr>
<tr>
<td>Last viewed product limit</td>
<td>
<p>silver_eshop.default.last_viewed_products_in_session_limit: 10</p>
<p><br />
</p>
</td>
<td>10</td>
<td>It defines the maximum number of products to be stored per user for last viewed products.</td>
</tr>
<tr>
<td>Product description character limit</td>
<td>silver_eshop.default.catalog_description_limit: 50</td>
<td>50</td>
<td>Sets the number of characters for product description in the list views / search results. When reached the description will be truncated. In order to remove the limit use really high number like 999999</td>
</tr>
</tbody>
</table>

## Price

Configure currencies and set price provider for the shop. Advanced version only Some configurations are only possible and important for eZ Commerce Advanced. The price provider "siso\_price.price\_provider.remote" is offered by Advanced version only.

The possibility to manage the source of prices for different situations in the shop  (remote price provider (=from ERP) and shop price provider) is heplful for performance reasons.  
<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 39%" />
<col style="width: 8%" />
<col style="width: 35%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Last currency rate change</td>
<td><br />
</td>
<td>empty</td>
<td>The value (date) describes, when the conversion rate has changed the last time. </td>
</tr>
<tr>
<td>Automatic currency conversion</td>
<td><br />
</td>
<td>False</td>
<td>In case no price is setup for this currency the shop can calculate the price using a conversion rate from the configuration.</td>
</tr>
<tr>
<td>Currency conversion rate<br />
</td>
<td><br />
</td>
<td><br />
</td>
<td>Define the conversion rate between currencies here.</td>
</tr>
<tr>
<td>Default currency</td>
<td>siso_core.default.standard_price_factory.fallback_currency: EUR</td>
<td>EUR</td>
<td>Used as currency for the shop (e.g. per siteaccess)</td>
</tr>
<tr>
<td>Base currency</td>
<td>siso_core.default.standard_price_factory.base_currency: EUR </td>
<td>EUR</td>
<td>Base currency of the shop in general (is used for the fields "product unit price" and "fallback shipping price". The base currency will be used for the automatic currency conversion)</td>
</tr>
<tr>
<td>Price providers for product listing page</td>
<td>
<p>siso_price.default.price_service_chain.product_list:</p>
<p>- siso_price.price_provider.shop</p>
<p>- siso_price.price_provider.remote</p>
</td>
<td>shop</td>
<td><p>Advanced version only<br />
Please choose which price calculation engines shall be used for generating prices and stock information.</p>
<p><strong>siso_price.price_provider.shop</strong> means price engine in the shop is used for price calculation.</p>
<p>Advanced version only<br />
</p>
<p><strong>siso_price.price_provider.remote</strong> means ERP is used for price calculation.</p>
<p>This configuration works as a chain, so if the first engine fails the second one will be used as a fallback (e.g. in case the ERP is not availbale)<br />
Important note: Price calculations in ERP systems may cause slower responses from the system, depending on the specific complexity.</p></td>
</tr>
<tr>
<td>Price providers for product detail page</td>
<td>
<p>siso_price.default.price_service_chain.product_detail:</p>
<p>- siso_price.price_provider.shop</p>
<p>- siso_price.price_provider.remote</p>
</td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for product sliders</td>
<td>
<p>siso_price.default.price_service_chain.slider.product_detail: </p>
<p>- siso_price.price_provider.shop </p>
<p>- siso_price.price_provider.remote<br />
</p>
<p><br />
</p>
</td>
<td><p>shop</p></td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for basket</td>
<td>
<p>siso_price.default.price_service_chain.basket:</p>
<p>- siso_price.price_provider.shop</p>
<p>- siso_price.price_provider.remote</p>
<p><br />
</p>
</td>
<td><p>shop</p></td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for editing variants</td>
<td>
siso_price.default.price_service_chain.basket_variant:
<p>- siso_price.price_provider.shop</p>
<p>- siso_price.price_provider.remote</p>
<p><br />
</p>
<p><br />
</p>
</td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for stored baskets lists</td>
<td>
<p>siso_price.default.price_service_chain.stored_basket:</p>
<p>- siso_price.price_provider.remote</p>
<p>- siso_price.price_provider.shop</p>
</td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for whishlist</td>
<td>
<p>siso_price.default.price_service_chain.wish_list:</p>
<p>- siso_price.price_provider.shop</p>
<p>- siso_price.price_provider.remote<br />
</p>
</td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for quickorder (click on update)</td>
<td>
<p>siso_price.default.price_service_chain.quick_order:<br />
- siso_price.price_provider.shop</p>
<p>- siso_price.price_provider.remote</p>
</td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for quickorder (product preview via Ajax)</td>
<td><p>siso_price.default.price_service_chain.quick_order_line_preview:</p>
<p>- siso_price.price_provider.shop</p>
- siso_price.price_provider.remote</td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for comparison lists</td>
<td>
<p>siso_price.default.price_service_chain.comparison:</p>
<p>- siso_price.price_provider.remote</p>
<p>- siso_price.price_provider.shop</p>
</td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price providers for search lists</td>
<td>
<p>siso_price.default.price_service_chain.search_list:</p>
<p>- siso_price.price_provider.remote</p>
<p>- siso_price.price_provider.shop</p>
<p><br />
</p>
</td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
<tr>
<td>Price provider for bestseller list</td>
<td><p>siso_price.default.price_service_chain.bestseller_list:</p>
<p>- siso_price.price_provider.shop</p>
<p>- siso_price.price_provider.remote<br />
</p></td>
<td>shop</td>
<td>Choose shop or remote</td>
</tr>
</tbody>
</table>

## Customer

**Note:** Olark chat tool requires an account with this third party provider.

<table style="width:100%;">
<colgroup>
<col style="width: 23%" />
<col style="width: 28%" />
<col style="width: 4%" />
<col style="width: 42%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Enable Olark Chat</td>
<td>
<p>siso_core.default.marketing.olark_chat.activated: false</p>
</td>
<td>True</td>
<td>Olark chat is a powerful chat tool. Please check <a href="http://www.olark.com" class="external-link">www.olark.com</a> for more information.</td>
</tr>
<tr>
<td>Please enter the Account ID from <a href="http://www.olark.com" class="external-link">www.olark.com</a> here</td>
<td><p>siso_core.default.marketing.olark_chat.id: '6295-386-10-7457'</p></td>
<td><br />
</td>
<td>Olark account id</td>
</tr>
</tbody>
</table>

## Advanced Catalog Features 

Choose advanced shop feature settings

<table style="width:100%;">
<colgroup>
<col style="width: 23%" />
<col style="width: 36%" />
<col style="width: 4%" />
<col style="width: 35%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Disable ordering for discontinued products</td>
<td>
<p>siso_basket.default.discontinued_products_listener_active: true</p>
</td>
<td>True</td>
<td>If enabled products which are discontinued and out of stock cannot be ordered any more.</td>
</tr>
<tr>
<td>Check packaging units and adjust quantity of discontinued products</td>
<td>
<p>siso_basket.default.discontinued_products_listener_consider_packaging_unit: true</p>
</td>
<td>True</td>
<td>If enabled the shop will modify the quantity of discontinued products ordered by a user automatically to the next packaging unit.</td>
</tr>
</tbody>
</table>

## Mail and Newsletter

**Note:** Newsletter2go requires the activation of a seperate plugin and an account with Newsletter2go.

<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Email address of a sales person</td>
<td><p>siso_eshop.default.orderconfirmation_sales_email_address:</p></td>
<td><br />
</td>
<td>This person will get an CC of an order confirmation email (for all orders generated in the shop)</td>
</tr>
<tr>
<td>Newsletter</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>Activate Newsletter2go</td>
<td>
<p>siso_newsletter.default.newsletter_active: false</p>
</td>
<td>False</td>
<td>You need to create an account on <a href="http://www.newsletter2go.com" class="external-link">www.newsletter2go.com</a></td>
</tr>
<tr>
<td>Unsubscribe from all newsletters</td>
<td><p>siso_newsletter.default.unsubscribe_globally: true</p></td>
<td>True</td>
<td>Unsubscribe options. If true shop users can unsubscribed from all newsletters in one step.</td>
</tr>
<tr>
<td>Newsletter only for logged in users</td>
<td>
<p>siso_newsletter.default.display_newsletter_box_for_logged_in_users: true</p>
</td>
<td>True</td>
<td>If enabled only logged in users will see the newsletter box.</td>
</tr>
<tr>
<td>Newsletter2go username</td>
<td>
<p>siso_newsletter.default.newsletter2go_username: ''</p>
</td>
<td>-</td>
<td>See newsletter2go account</td>
</tr>
<tr>
<td>Newsletter2go password</td>
<td>
<p>siso_newsletter.default.newsletter2go_password: ''</p>
</td>
<td>- </td>
<td>See newsletter2go account</td>
</tr>
<tr>
<td>Newsletter2go authentication key</td>
<td>
<p>siso_newsletter.default.newsletter2go_auth_key: ''</p>
</td>
<td>-</td>
<td>This key is available in your newsletter2go account</td>
</tr>
</tbody>
</table>

## Basket

Defines detailed behaviour for the basket.

<table style="width:100%;">
<colgroup>
<col style="width: 20%" />
<col style="width: 40%" />
<col style="width: 4%" />
<col style="width: 34%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Duration of storing anonymous baskets</td>
<td>
<p>ses_basket.default.validHours: 120</p>
</td>
<td><a href="mailto:fd@silversolutions.de" class="external-link">1</a>20</td>
<td>Defines how many hours anonymous baskets are stored.</td>
</tr>
<tr>
<td>Update product data after this time</td>
<td>ses_basket.default.refreshCatalogElementAfter: '1 hours'</td>
<td>1 hours</td>
<td>Product data is cached in the basket for a given time. Please use a syntax "1 hour".</td>
</tr>
<tr>
<td>Display stock as a column</td>
<td>
<p>ses_basket.default.stock_in_column: true</p>
<p><br />
</p>
</td>
<td>True</td>
<td>True - display in column, False - display inline (in product name column).</td>
</tr>
<tr>
<td>Description character limit</td>
<td>
<p>ses_basket.default.description_limit: 50</p>
</td>
<td>50</td>
<td>Number of characters that will be visible for the description field.</td>
</tr>
<tr>
<td>Enable additional comment line for basket</td>
<td>
<p>ses_basket.default.additional_text_for_basket_line: false</p>
<p><br />
</p>
</td>
<td>False</td>
<td>If enabled the customer can enter a comment to each line in the basket or quickorder.</td>
</tr>
<tr>
<td>Max. chars for the additional comment line</td>
<td>
<p>ses_basket.default.additional_text_for_basket_line_input_limit: 30</p>
<p><br />
</p>
</td>
<td>30</td>
<td><p>Advanced version only</p>
<p>Important note: the ERP often has a restriction and might not support long comments. This might lead to errors during order process</p></td>
</tr>
</tbody>
</table>

## Stored basket

Configures the displayed product data for stored baskets.

<table style="width:100%;">
<colgroup>
<col style="width: 13%" />
<col style="width: 29%" />
<col style="width: 4%" />
<col style="width: 52%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Display stock as a column</td>
<td>
<p>ses_stored_basket.default.stock_in_column: true</p>
</td>
<td>True</td>
<td>True - display in column, False - display inline (inside product name column).</td>
</tr>
<tr>
<td>Description character limit</td>
<td>
<p>ses_stored_basket.default.description_limit: 50</p>
</td>
<td>50</td>
<td>Number of characters that will be visible for the description field.</td>
</tr>
</tbody>
</table>

## Wishlist

<table style="width:100%;">
<colgroup>
<col style="width: 13%" />
<col style="width: 19%" />
<col style="width: 4%" />
<col style="width: 62%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Description character limit</td>
<td>
<p>ses_wishlist.default.description_limit: 50</p>
<p><br />
</p>
</td>
<td>50</td>
<td>Number of characters that will be visible for the description field</td>
</tr>
</tbody>
</table>

## ERP (only if shop is connected to ERP)

Defines the details of data exchange and processes between your shop and your ERP.  ERP integration requires a web.connector licence or another webservice interface between ERP and Shop.

<table>
<colgroup>
<col style="width: 22%" />
<col style="width: 40%" />
<col style="width: 12%" />
<col style="width: 24%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Default Country for Template Debitor</td>
<td>
<p>siso_core.default.template_debitor_country: DE</p>
<p><br />
</p>
</td>
<td>DE</td>
<td>Determines which country shall be used as a default.<br />
Please use the country code such as "DE" for Germany.</td>
</tr>
<tr>
<td>Login with Customer Number</td>
<td>
<p>siso_core.default.enable_customer_number_login: false</p>
</td>
<td>False</td>
<td>This true|false value determines, if the login process should include a field for providing customer number.</td>
</tr>
<tr>
<td>Use a template debitor number for this shop</td>
<td>
<p>siso_core.default.use_template_debitor_customer_number: true</p>
<p><br />
</p>
</td>
<td>True</td>
<td>A template debitor customer number is used, if a customer does not have a customer number from the ERP.<br />
The template debitor customer numbers can be defined for each country.</td>
</tr>
<tr>
<td>Use a template contact number for this shop</td>
<td>
<p>siso_core.default.use_template_debitor_contact_number: true</p>
<p><br />
</p>
</td>
<td>True</td>
<td>A template contact number is used if a customer does not have a customer and contact number from the ERP.<br />
The template contact numbers can be defined for each country</td>
</tr>
<tr>
<td>price_requests_without_customer_number|config</td>
<td><p>siso_core.default.price_requests_without_customer_number: true</p></td>
<td>True</td>
<td>This true|false value determines, if a price request will be sent to the ERP without customer number. A template debitor will be used to calculate prices.</td>
</tr>
<tr>
<td>Recalculate prices using the ERP after</td>
<td>ses_basket.default.recalculatePricesAfter: '3 hours'</td>
<td>True</td>
<td>Information from the ERP is cached in order to reduce the traffic towards the ERP. Please use a syntax "1 hour".</td>
</tr>
<tr>
<td><p>Variants handling in the ERP</p></td>
<td>
<p>silver_eshop.default.erp.variant_handling: SKU_ONLY</p>
<p><br />
</p>
</td>
<td>SKU_ONLY</td>
<td>Choose which handling of variant products should be taken.<br />

<ul>
<li><strong>SKU_ONLY</strong> - if your ERP system uses different SKUs per variant.</li>
<li><strong>SKU_AND_VARIANT</strong> - if your ERP system uses a combination of SKU and variant code for variants.</li>
</ul></td>
</tr>
<tr>
<td>URL of the Web-Connector</td>
<td>siso_erp.default.web_connector.service_location: </td>
<td>-</td>
<td>The URL points to your Web-Connector System installed near by your ERP system. Please use a https connection and make sure that the shop can access this ip and port only!</td>
</tr>
<tr>
<td>User name (configured per Web-Connector)</td>
<td>
<p>silver_eshop.default.webconnector.username: admin</p>
</td>
<td>admin</td>
<td>User name for the communication with the Web-Connector service</td>
</tr>
<tr>
<td>Password (configured per Web-Connector)</td>
<td>
<p>silver_eshop.default.webconnector.password: passwo</p>
</td>
<td>passwo</td>
<td>Password for the communication with the Web-Connector service</td>
</tr>
<tr>
<td>SOAP Web-Service timeout in seconds</td>
<td>
<p>silver_eshop.default.webconnector.soapTimeout: 5</p>
</td>
<td>5</td>
<td>Timeout (Web-Service) for the communication with the Web-Connector in seconds</td>
</tr>
<tr>
<td>Timeout towards the ERP-System in seconds</td>
<td>
<p>silver_eshop.default.webconnector.erpTimeout: 5</p>
</td>
<td>5</td>
<td>Timeout (ERP) for the communication with the Web-Connector in seconds</td>
</tr>
</tbody>
</table>

##   
## Miscellaneous

Define settings for additional features

<table style="width:100%;">
<colgroup>
<col style="width: 24%" />
<col style="width: 27%" />
<col style="width: 9%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Number of bestsellers displayed on bestseller page</td>
<td>
<p>siso_core.default.bestseller_limit_on_bestseller_page: 6</p>
</td>
<td>6</td>
<td>Choose the limit of bestsellers that should be displayed.</td>
</tr>
<tr>
<td>Number of bestsellers displayed on catalog pages</td>
<td>siso_core.default.bestseller_limit_on_catalog_page: 6</td>
<td>6</td>
<td>Choose the limit of bestsellers that should be displayed.</td>
</tr>
<tr>
<td>Number of bestsellers displayed in a slider</td>
<td>siso_core.default.bestseller_limit_in_silver_module: 6</td>
<td>6</td>
<td>Choose the limit of bestsellers that should be displayed.</td>
</tr>
<tr>
<td>Threshold bestseller</td>
<td>siso_core.default.bestseller_threshold: 1</td>
<td>1</td>
<td>How often has a product to be bought, to count as a bestseller?</td>
</tr>
</tbody>
</table>

## Checkout

Configure your payment and shipping methods. Paypal requires an account.

<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Payment method "PayPal"<br />
</td>
<td>
<p>siso_checkout.default.payment_method.paypal_express_checkout: true</p>
</td>
<td>True</td>
<td>Enables "PayPal" in checkout<br />
</td>
</tr>
<tr>
<td>Payment method "invoice"</td>
<td>siso_checkout.default.payment_method.invoice: true</td>
<td>True</td>
<td>Enables "invoice" in checkout</td>
</tr>
<tr>
<td>Shipping method "standard"</td>
<td>siso_checkout.default.shipping_method.standard: true</td>
<td>True</td>
<td>Enables shipping method "standard" in checkout</td>
</tr>
<tr>
<td>Shipping method "express"</td>
<td>siso_checkout.default.shipping_method.express_delivery: true</td>
<td>True</td>
<td>Enables shipping method "express" in checkout</td>
</tr>
</tbody>
</table>

## Fallback configuration for price engine

Fallback configuration for price engine. It is used, if no shipping costs are set in [price and stock management](Manage-prices-and-stock_23560467.html).

<table style="width:100%;">
<colgroup>
<col style="width: 23%" />
<col style="width: 30%" />
<col style="width: 5%" />
<col style="width: 40%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Fallback costs for shipping</td>
<td>
<p>siso_local_order_management.default.shipping_cost: ''</p>
</td>
<td>-</td>
<td>Shipping cost for shop in shop currency if shipping free limit is not reached<br />
</td>
</tr>
<tr>
<td>No shipping costs for orders from amount</td>
<td>siso_local_order_management.default.shipping_free: ''</td>
<td>-</td>
<td>Shipping free limit for shop in shop currency</td>
</tr>
<tr>
<td>Fallback VAT Code for shipping costs</td>
<td>siso_core.default.shipping_vat_code: '19'</td>
<td>19</td>
<td>Used for calculating the VAT part for shipping</td>
</tr>
</tbody>
</table>

## Configuration for countries/site accesses

For some settings the shop requires a country specific configuration.

![](attachments/23561024/23571063.jpg)

## Price

Configure currencies and set price provider for the shop.  
<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Default currency<br />
</td>
<td>
<p>siso_core.en.standard_price_factory.fallback_currency: USD</p>
</td>
<td>USD</td>
<td>This currency is used as fallback currency, if the siteaccess specific configuration is not configured (e.g. per siteaccess)<br />
</td>
</tr>
</tbody>
</table>

## Checkout

Configure your payment and shipping methods. Paypal requires an account.

<table style="width:100%;">
<colgroup>
<col style="width: 19%" />
<col style="width: 42%" />
<col style="width: 6%" />
<col style="width: 31%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Stored Configuration File</th>
<th>Default</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Payment method "PayPal"<br />
</td>
<td>
<p>siso_checkout.en.payment_method.paypal_express_checkout: true</p>
</td>
<td>True</td>
<td>Enables "PayPal" in checkout<br />
</td>
</tr>
<tr>
<td>Payment method "invoice"</td>
<td>siso_checkout.en.payment_method.invoice: true</td>
<td>True</td>
<td>Enables "invoice" in checkout</td>
</tr>
<tr>
<td>Shipping method "standard"</td>
<td>siso_checkout.default.en.shipping_method.standard: true</td>
<td>True</td>
<td>Enables shipping method "standard" in checkout</td>
</tr>
<tr>
<td>Shipping method "express"</td>
<td>siso_checkout.default.en.shipping_method.express_delivery: true</td>
<td>True</td>
<td>Enables shipping method "express" in checkout </td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [Screen Shot 2016-07-13 at 16.08.47.png](attachments/23561024/23563963.png) (image/png)  
![](images/icons/bullet_blue.gif) [configuration-settings.jpg](attachments/23561024/23563965.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [configuration-settings.jpg](attachments/23561024/23563964.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Configuration settings\_silver.eShop.png](attachments/23561024/23571011.png) (image/png)  
![](images/icons/bullet_blue.gif) [Configuration settings.png](attachments/23561024/23570785.png) (image/png)  
![](images/icons/bullet_blue.gif) [recaptcha.png](attachments/23561024/23571012.png) (image/png)  
![](images/icons/bullet_blue.gif) [config\_productlist.png](attachments/23561024/23571015.png) (image/png)  
![](images/icons/bullet_blue.gif) [config\_productlist.png](attachments/23561024/23571014.png) (image/png)  
![](images/icons/bullet_blue.gif) [Configuration settings.jpg](attachments/23561024/23571066.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Configuration settings\_EN.jpg](attachments/23561024/23571052.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Configuration settings\_EN.jpg](attachments/23561024/23571054.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Configuration settings\_EN.jpg](attachments/23561024/23571063.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Configuration settings\_DE.jpg](attachments/23561024/23571056.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [Configuration settings.png](attachments/23561024/23571013.png) (image/png)  
