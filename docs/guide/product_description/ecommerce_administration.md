# eCommerce administration

eZ Commerce comes with a convenient configuration and management tools for the shop owner.

![](img/Backend.png)

## Cockpit

The cockpit provides a good overview about the latest shop statistics, e.g. for sales or orders. It does not replace tools such as google analytics.

- Number of customers and active shopping carts
- Top sellers
- Top customers
- Charts: orders and sales for the selected date(s)
- Search statistics

Advanced version only

- Status of the ERP system
- Status of the server

![](img/Cockpit.png)

## Components

In the components section the shop admin will find the "Textmodules".

![](img/Components_subitems.png)

The item silver.eShop gives an overview of the currently set configuration.

![](img/Components_silvereshop.png)

See configurations can be changed in the configuration setting (see below).

In the item Textmodules all text concerning the shop can be managed as for basket, checkout, contact, customer center (if activated), email, footer, forms, order history and user.

![](img/Components_Textmoduls.png)

## Order management

Orders are stored in the backend and can be filtered by date, status or payment information. An export function allows to export the data as XML file.

![](img/Order_Management.png)

If the shop is used without ERP integration invoices can be managed as well.

## Configuration
	
The configuration settings in the backend allow to configure the main behaviour for the shop. In addition currency, payment and shipping options can be set per country (siteaccess). The configuration is stored using the version control features of the CMS.  More advanced settings (i.e. VAT for different countries) can be set up in configuration files.

![](img/Configuration_settings.png)

## Price and shiping cost management

The Price and shiping costs management offers to setup prices and stock in case when the shop price provider (siso_price.price_provider.shop) is configured for calculating prices.

![](img/Product_price_management.png)

See more details in the section [Manage products, prices and stock](manage_products_prices_and_stock.md).
