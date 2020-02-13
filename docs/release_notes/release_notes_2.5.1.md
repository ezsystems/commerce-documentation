# Release notes 2.5.1 (4.1.2) 
ses-brand version 2.5.1 (4.1.2)

  - intermediate version
  - supported eZ Platform Version V2.5
 If you apply this %ses-brand version in the project, always check the latest tagged version.

Server requirements%ses-brand relies on eZ Publish software that is built to rely on existing technologies and standards, mainly:

  - PHP scripting language: 7
  - SQL database: *MySql/MariaDB or PostgreSQL*
  - Web Server: Apache/Nginx
  - Java JRE  (Oracle-Sun/OpenJDK) when Solr is used (for use with eZ Find search engine)

## New features and improvements

List of new features and improvements.

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/17084" class="external-link">#17084</a></td>
<td>Basket</td>
<td><p><span style="color: rgb(0,0,0);">The modified flag was set if catalogElement was updated although customer did not update the basket.</p>
<p><span style="color: rgb(0,0,0);">The "storeBasket" method from BasketService does not update dateLastModified <span style="color: rgb(0,0,0);">anymore, if called by</p>
<p><span style="color: rgb(0,0,0);">handleFetchedBasket method (new flag $updateDateLastModified).</p></td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18142" class="external-link">#18142</a></td>
<td><p><span style="color: rgb(0,0,0);">Dataprocessors</p></td>
<td><p><span style="color: rgb(0,0,0);">Add backwards compatibility for older projects to dataprocessors.</p></td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/16881" class="external-link">#16881</a></td>
<td>New FieldType product relation</td>
<td>This Fieldtype offers a relation feature for products. It can be used with product stored in eZ or in eContent.</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/16289" class="external-link">#</a><a href="http://rm.extranet.silversolutions.de/issues/16289" class="external-link">16289</a></td>
<td>Improved caching for basket preview</td>
<td><a href="http://rm.extranet.silversolutions.de/issues/16289" class="external-link">I</a>mproved caching implementation for dynamic and user specific data and used first for basket preview</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/17575" class="external-link">#17575</a></td>
<td>Quantity via +/- button</td>
<td>It is possible to add button for +/- to increase/decrease the quantity.</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18351" class="external-link">#18351</a></td>
<td>Product search</td>
<td>Added a new <a href="Use-query-filter-in-product-search_29818895.html">filter condition</a> in order to use Solr query filters</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/16289" class="external-link">#16289</a></td>
<td>Ajax call for customer and basket data</td>
<td>An ajax call can get data from customer and basket</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/17740" class="external-link">#17740</a></td>
<td>Lost order</td>
<td><p>New lost order features:</p>
<ul>
<li>Link to lost orders in backend was added to lost order mail</li>
<li>Two new REST calls were added for lost order in the backend:
<ul>
<li>Transfer lost order to erp</li>
<li>Remove lost order</li>
</ul></li>
</ul></td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18307" class="external-link">#18307</a></td>
<td>configuration for search - used econtent core</td>
<td>This feature enables to use a dedicated econtent core for a siteaccess (e.g. for a preview siteaccess using the back core of econtent)</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18424" class="external-link">#18424</a></td>
<td>New JS trigger</td>
<td>New trigger in JS after a product list is loaded via ajax: $(document).trigger('getSearchResult', [response]);</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18365" class="external-link">#18365</a></td>
<td>Solr</td>
<td><p><span style="color: rgb(72,72,72);">Added plugin to index names in ascii format (umlaut sort problem)</p></td>
</tr>
</tbody>
</table>

## Bug fixes

List of bugs that were fixed in this version.

<table>
<colgroup>
<col style="width: 27%" />
<col style="width: 26%" />
<col style="width: 46%" />
</colgroup>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
<a href="http://rm.extranet.silversolutions.de/issues/18059" class="external-link">#18059</a><br />

</td>
<td>PaymentBundles</td>
<td><p>Replace outdated configuration key 'pattern' with 'path'</p></td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18085" class="external-link">#18085</a></td>
<td>Various bugs</td>
<td><p>Fix bugs in <span style="color: rgb(72,72,72);">SilvercommonExtension, <span style="color: rgb(72,72,72);">profiler configuration and composer.json</p></td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18084" class="external-link">#18084</a></td>
<td><p>SesExternalData</p></td>
<td><ul>
<li><span style="color: rgb(72,72,72);">Add serialization and deserialization</li>
<li><span style="color: rgb(72,72,72);">Add missing logger service</li>
<li><span style="color: rgb(72,72,72);">Add field template</li>
<li><span style="color: rgb(72,72,72);">Requires a new field in the ses_externaldata table: ses_sku_erp (TextLineField)</li>
</ul></td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18005" class="external-link">#18005</a></td>
<td><p>Policies</p></td>
<td>Remove duplicated tilde which was causing a bug when the policy "<span style="color: rgb(72,72,72);">lostorder_list" was asigned to a role</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18115" class="external-link">#18115</a></td>
<td>Login</td>
<td><p>Fix login with customer number that was not working</p></td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18134" class="external-link">#18134</a></td>
<td>Logout</td>
<td>Fix error on logout action, when session is outdated or gone</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/17935" class="external-link">#17935</a></td>
<td>Newsletter</td>
<td>Newsletter Block in Page Builder is only available when the newsletter feature is active</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18215" class="external-link">#18215</a></td>
<td>Breadcrumbs</td>
<td>Handle exception during rendering of bread crumbs</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18272" class="external-link">#18272</a></td>
<td>Exception handling</td>
<td>Catch endless loop caused by errors</td>
</tr>
<tr class="even">
<td>#<a href="http://rm.extranet.silversolutions.de/issues/18333" class="external-link">18333</a></td>
<td>Remove cookie policy location configuration</td>
<td><div class="content-wrapper">
<p>Remove configuration</p>
<div class="code panel pdl" style="border-width: 1px;">
<div class="codeContent panelContent pdl">
<pre class="syntaxhighlighter-pre" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>silver_eshop.default.cookie_policy_location_id</code></pre>
</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/17249" class="external-link">#17249</a></td>
<td>Content urls not accessable</td>
<td>in case econtent does not contain products content (empty sve_object table) urls were not accessible</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/17302" class="external-link">#17302</a></td>
<td>Shippingcosts</td>
<td>If no shipping costs could be determined no shipping costs line will be added.</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18441" class="external-link">#18441</a></td>
<td>Login</td>
<td>Redirect to wrong URI after login</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18410" class="external-link">#18410</a></td>
<td>Registration</td>
<td>set vat paramteres for business and private registration</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18475" class="external-link">#18475</a></td>
<td>Product list pagination</td>
<td>In some use cases browse to next page was not working.</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18395" class="external-link">#18395</a></td>
<td>Quickorder change sku</td>
<td>It was not possible to change sku in existing quickorder basket. Sku was not overwritten.</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18004" class="external-link">#18004</a></td>
<td><p>Admin price management</p></td>
<td><p>Admin price management was not using proper route</p></td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/17948" class="external-link">#17948</a></td>
<td>Customer profile data</td>
<td><p>Remove unnecessary class variable which was not always up to date</p></td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/17878" class="external-link">#17878</a></td>
<td>Page builder</td>
<td>Remove two unused layouts, images and templates.</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/17761" class="external-link">#17761</a></td>
<td>ERP listener</td>
<td>ERP listener was overwriting data even if ERP was offline</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18319" class="external-link">#18319</a></td>
<td>Login</td>
<td>Not all login forms offered login with customer number</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18164" class="external-link">#18164</a></td>
<td>Checkout</td>
<td>Missing translation added</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18420" class="external-link">#18420</a></td>
<td>Invoice</td>
<td>Use design engine to determine the invoice template.</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18421" class="external-link">#18421</a></td>
<td>Forms</td>
<td>Fix bugs after switch to Bootstrap 4</td>
</tr>
</tbody>
</table>

## Tagged versions

List of tagged versions. These tags are created in the next development phase - 2.5.1++. This list must be fulfilled when the next release was published.

### silver.eShop

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
<th>Date</th>
<th>Tag</th>
<th>Patch</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
<a href="http://rm.extranet.silversolutions.de/issues/18627" class="external-link">#18627</a>
</td>
<td><p>EzUserHelper</p></td>
<td><p>Deprecated/Removed criterion still used in EzUserHelper</p></td>
<td><div class="content-wrapper">
<p>01 Nov 2019 </p>
</td>
<td>v4.1.2.1</td>
<td><div class="content-wrapper">
<p><a href="attachments/29819755/29821301.patch">hotfix-v4.1.2-1-18627.patch</a></p>
</td>
</tr>
<tr class="even">
<td><a href="http://rm.extranet.silversolutions.de/issues/18650" class="external-link">#18650</a></td>
<td>AuthenticationProvider</td>
<td><p>AuthenticationProvider not using correct Interface</p></td>
<td><div class="content-wrapper">
<p>08 Nov 2019 </p>
</td>
<td>v4.1.2.2</td>
<td><div class="content-wrapper">
<p><a href="attachments/29819755/29821296.patch">hotfix-v4.1.2-2-18650.patch</a></p>
</td>
</tr>
<tr class="odd">
<td><a href="http://rm.extranet.silversolutions.de/issues/18733" class="external-link">#18733</a></td>
<td><p>OlarkService</p></td>
<td>Wrong typehint used</td>
<td><div class="content-wrapper">
<p>21 Nov 2019 </p>
</td>
<td>v4.1.2.3</td>
<td><div class="content-wrapper">
<p><a href="attachments/29819755/29821290.patch">hotfix-v4.1.2-3-18733.patch</a></p>
</td>
</tr>
</tbody>
</table>

### silver.customercenter

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
<th>Date</th>
<th>Tag</th>
<th>Patch</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
#12345<br />

</td>
<td>Example</td>
<td>fixed a bug</td>
<td>23-02-15</td>
<td>3.1-1234</td>
<td><div class="content-wrapper">
<div class="content-wrapper">
<p><a href="#" class="unresolved">EZP-27011-solr-index-location-change.patch</a><br />
</p>

</td>
</tr>
</tbody>
</table>

### silver.orderhistory

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
<th>Date</th>
<th>Tag</th>
<th>Patch</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><div class="content-wrapper">
#12345<br />

</td>
<td>Example</td>
<td>fixed a bug</td>
<td>23-02-15</td>
<td>3.1-1234</td>
<td><div class="content-wrapper">
<div class="content-wrapper">
<p><a href="#" class="unresolved">EZP-27011-solr-index-location-change.patch</a><br />
</p>

</td>
</tr>
</tbody>
</table>
# API Changes

## silver.eShop

### Services

#### Constructor changes

added parameter Monolog\\Logger $logging in constructor for these Services:
```
src/Silversolutions/Bundle/EshopBundle/EventListener/Basket/DiscontinuedProductsListener.php
src/Silversolutions/Bundle/EshopBundle/Service/CustomerSkuService.php
src/Silversolutions/Bundle/EshopBundle/Service/Security/UserProvider.php
src/Silversolutions/Bundle/EshopBundle/Services/CatalogElementSerializeService.php
src/Silversolutions/Bundle/EshopBundle/Services/DataProvider/Econtent/EcontentAccess.php
src/Silversolutions/Bundle/EshopBundle/Services/StandardBasketListener.php
src/Siso/Bundle/LocalOrderManagementBundle/EventListener/OrderConfirmationListener.php
src/Siso/Bundle/LocalOrderManagementBundle/Service/InvoiceService.php
src/Siso/Bundle/NewsletterBundle/Service/DataProcessor/SubscribeNewsletterDataProcessor.php
src/Siso/Bundle/NewsletterBundle/Service/DataProcessor/UpdateNewsletterDataProcessor.php
src/Siso/Bundle/SearchBundle/Service/EzProductTypeIndexerPlugin.php
src/Siso/Bundle/SearchBundle/Service/EzVisibilityPlugin.php
src/Silversolutions/Bundle/EshopBundle/Service/CatalogHelper.php
src/Silversolutions/Bundle/EshopBundle/Service/GeneratePDFService.php
src/Silversolutions/Bundle/EshopBundle/Services/DataProvider/Ez5CatalogDataProvider.php
src/Siso/Bundle/NewsletterBundle/EventListener/UpdateNewsletterEventListener.php
src/Siso/Bundle/SearchBundle/Service/Ezp/CatalogVisibilityFieldMapper.php
```

###   
Method signature changes

>New parameter $updateDateLastModified was added to the BasketService method 'storeBasket' in Order to determine whether dateLastModified should be updated or not:
**src/Silversolutions/Bundle/EshopBundle/Services/BasketService.php** 

```
- public function storeBasket(Basket $basket)
+ public function storeBasket(Basket $basket, $updateDateLastModified = true)
```

### Interface changes

**BasketServiceInterface.php**

``` 
- public function storeBasket(Basket $basket);
+ public function storeBasket(Basket $basket, $updateDateLastModified = true);
```
### Templates

**Removed templates**

```
src/Siso/Bundle/EzStudioBundle/Resources/views/layouts/2_1.html.twig
src/Siso/Bundle/EzStudioBundle/Resources/views/layouts/2_1__1.html.twig
src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/Form/fields.html.twig
src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/fielddefinition/edit/sesproductselectiontype.html.twig
src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/fielddefinition/view/sesproductselectiontype.html.twig
src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/fieldtypes/sesproductselectiontype.html.twig
```

**Changed templates**
```
src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/sesproductselectiontype_content_field.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/blocks/basket.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout_footer.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/parts/header_login.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Admin/order_list.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Admin/tabs/product_ecommerce.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productBasket.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productDetailButtonGroup.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/product.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_delivery_address.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_invoice_address.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_shipping_payment.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_summary.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Emails/NotificationMail_FailedOrder.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Exception/exception.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/activate_business.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/contact.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/delivery_address.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/form_div_layout.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/my_account.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/party.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/password_change.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/password_reminder.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/register_business.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/register_private.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/QuickOrder/quick_order_form.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/QuickOrder/quick_order_line.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Security/login.html.twig
src/Silversolutions/Bundle/EshopBundle/Resources/views/Thread/comment_new_content.html.twig
```
## silver.orderhistory
Follow these changes only, if you are using [Orderhistory](#) in your project.

#### Constructor changes

src/Siso/Bundle/OrderHistoryBundle/Services/DateTimeService.php

### Templates

**Changed templates** 
```
src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/fields.html.twig
src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/detail.html.twig
```

## silver.customercenter
Follow these changes only, if you are using [Customercenter](#) in your project.

###   
Interface changes
**FormProcessorInterface**

``` 
- public function execute(Form $form, array $lastResult = array());
+ public function execute(FormInterface $form, array $lastResult = array());
```

### Constructor changes

Symfony\\Bundle\\FrameworkBundle\\Templating\\DelegatingEngine in constructor replaced by Symfony\\<span style="font-family: Arial , sans-serif;">Component\\Templating\\EngineInterface:

```
src/Siso/Bundle/CustomerCenterBundle/EventListener/CheckBudgetEventListener.php
src/Siso/Bundle/CustomerCenterBundle/EventListener/RemoveBasketListener.php
```

### Templates

  - Templates have changed in the following location (Legacy code was refactored):  
    <span style="color: rgb(72,72,72);">src/Siso/Bundle/CustomerCenterBundle/Resources/views/CustomerCenter/Emails

### Configuration changes

  - Policy prefix has changed from '**s****es\_**' to '**s****iso\_**' <span style="color: rgb(72,72,72);">according to naming schema in silver-eShop policies.

  - Fix deprecated routing key (pattern -\> path). An example for all changes.
    
    
    **routing.yml** 
    
    ```
    - pattern:  /customer-center
    + path:  /customer-center
    ```
  - Fully-Quallified Class Names instead of type names:
    
    
    **customercenter.yml** 
    
    ``` 
    - type: choice
    + type: Symfony\Component\Form\Extension\Core\Type\ChoiceType
    
    
    - type: text
    + type: Symfony\Component\Form\Extension\Core\Type\TextType
    
    
    ...
    ```
## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above\!

## Attachments:

![](images/icons/bullet_blue.gif) [hotfix-v4.1.2-1-18627.patch](attachments/29819755/29821301.patch) (text/plain)  
![](images/icons/bullet_blue.gif) [v4.1.2-2-18650.patch](attachments/29819755/29821297.patch) (text/plain)  
![](images/icons/bullet_blue.gif) [hotfix-v4.1.2-2-18650.patch](attachments/29819755/29821296.patch) (text/plain)  
![](images/icons/bullet_blue.gif) [hotfix-v4.1.2-3-18650.patch](attachments/29819755/29821291.patch) (text/plain)  
![](images/icons/bullet_blue.gif) [hotfix-v4.1.2-3-18733.patch](attachments/29819755/29821290.patch) (text/plain)  
