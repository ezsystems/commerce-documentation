# Release notes 2.5.1 (4.1.2) 

eZ Commerce version 2.5.1 (4.1.2)

- intermediate version
- supported eZ Platform v2.5

If you apply this eZ Commerce version in the project, always check the latest tagged version.

Server requirements. eZ Commerce relies on eZ Platform software that is built to rely on existing technologies and standards, mainly:

- PHP scripting language: 7
- SQL database: *MySql/MariaDB or PostgreSQL*
- Web Server: Apache/Nginx
- Java JRE  (Oracle-Sun/OpenJDK) when Solr is used (for use with eZ Find search engine)

## New features and improvements

List of new features and improvements.

|Ticket|Topic|Description|
|--- |--- |--- |
|#17084|Basket|The modified flag was set if catalogElement was updated although customer did not update the basket.</br>The "storeBasket" method from BasketService does not update dateLastModified anymore, if called by handleFetchedBasket method (new flag $updateDateLastModified).|
|#18142|Dataprocessors|Add backwards compatibility for older projects to dataprocessors.|
|#16881|New FieldType product relation|This Fieldtype offers a relation feature for products. It can be used with product stored in eZ or in eContent.|
|#16289|Improved caching for basket preview|Improved caching implementation for dynamic and user specific data and used first for basket preview|
|#17575|Quantity via +/- button|It is possible to add button for +/- to increase/decrease the quantity.|
|#18351|Product search|Added a new [filter condition](../guide/search/search_cookbook/use_query_filter_in_product_search.md) in order to use Solr query filters|
|#16289|Ajax call for customer and basket data|An ajax call can get data from customer and basket|
|#17740|Lost order|New lost order features:</br>Link to lost orders in backend was added to lost order mail</br>Two new REST calls were added for lost order in the backend:</br>Transfer lost order to erp</br>Remove lost order|
|#18307|configuration for search - used econtent core|This feature enables to use a dedicated econtent core for a siteaccess (e.g. for a preview siteaccess using the back core of econtent)|
|#18424|New JS trigger|New trigger in JS after a product list is loaded via ajax: $(document).trigger('getSearchResult', [response]);|
|#18365|Solr|Added plugin to index names in ascii format (umlaut sort problem)|

## Bug fixes

List of bugs that were fixed in this version.

|Ticket|Topic|Description|
|--- |--- |--- |
|#18059|PaymentBundles|Replace outdated configuration key 'pattern' with 'path'|
|#18085|Various bugs|Fix bugs in SilvercommonExtension, profiler configuration and composer.json|
|#18084|SesExternalData|Add serialization and deserialization</br>Add missing logger service</br>Add field template</br>Requires a new field in the ses_externaldata table: ses_sku_erp (TextLineField)|
|#18005|Policies|Remove duplicated tilde which was causing a bug when the policy "lostorder_list" was asigned to a role|
|#18115|Login|Fix login with customer number that was not working|
|#18134|Logout|Fix error on logout action, when session is outdated or gone|
|#17935|Newsletter|Newsletter Block in Page Builder is only available when the newsletter feature is active|
|#18215|Breadcrumbs|Handle exception during rendering of bread crumbs|
|#18272|Exception handling|Catch endless loop caused by errors|
|#18333|Remove cookie policy location configuration|Remove configuration</br>`silver_eshop.default.cookie_policy_location_id`|
|#17249|Content urls not accessable|in case econtent does not contain products content (empty sve_object table) urls were not accessible|
|#17302|Shippingcosts|If no shipping costs could be determined no shipping costs line will be added.|
|#18441|Login|Redirect to wrong URI after login|
|#18410|Registration|set vat paramteres for business and private registration|
|#18475|Product list pagination|In some use cases browse to next page was not working.|
|#18395|Quickorder change sku|It was not possible to change sku in existing quickorder basket. Sku was not overwritten.|
|#18004|Admin price management|Admin price management was not using proper route|
|#17948|Customer profile data|Remove unnecessary class variable which was not always up to date|
|#17878|Page builder|Remove two unused layouts, images and templates.|
|#17761|ERP listener|ERP listener was overwriting data even if ERP was offline|
|#18319|Login|Not all login forms offered login with customer number|
|#18164|Checkout|Missing translation added|
|#18420|Invoice|Use design engine to determine the invoice template.|
|#18421|Forms|Fix bugs after switch to Bootstrap 4|


## Tagged versions

List of tagged versions. These tags are created in the next development phase - 2.5.1++. This list must be fulfilled when the next release was published.

### silver.eShop

|Ticket|Topic|Description|Date|Tag|Patch|
|--- |--- |--- |--- |--- |--- |
|#18627|EzUserHelper|Deprecated/Removed criterion still used in EzUserHelper|01 Nov 2019|v4.1.2.1|[hotfix-v4.1.2-1-18627.patch](img/hotfix-v4.1.2-1-18627.patch)|
|#18650|AuthenticationProvider|AuthenticationProvider not using correct Interface|08 Nov 2019|v4.1.2.2|[hotfix-v4.1.2-2-18650.patch](img/hotfix-v4.1.2-2-18650.patch)|
|#18733|OlarkService|Wrong typehint used|21 Nov 2019|v4.1.2.3|[hotfix-v4.1.2-3-18733.patch](img/hotfix-v4.1.2-3-18733.patch)|

# API Changes

## silver.eShop

### Services

#### Constructor changes

added parameter `Monolog\Logger $logging` in constructor for these services:

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

### Method signature changes

New parameter $updateDateLastModified was added to the BasketService method 'storeBasket' in Order to determine whether dateLastModified should be updated or not:

??? "src/Silversolutions/Bundle/EshopBundle/Services/BasketService.php"

    ```
    - public function storeBasket(Basket $basket)
    + public function storeBasket(Basket $basket, $updateDateLastModified = true)
    ```

### Interface changes

??? "BasketServiceInterface.php"

    ``` 
    - public function storeBasket(Basket $basket);
    + public function storeBasket(Basket $basket, $updateDateLastModified = true);
    ```
    
### Templates

??? "Removed templates"

    ```
    src/Siso/Bundle/EzStudioBundle/Resources/views/layouts/2_1.html.twig
    src/Siso/Bundle/EzStudioBundle/Resources/views/layouts/2_1__1.html.twig
    src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/Form/fields.html.twig
    src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/fielddefinition/edit/sesproductselectiontype.html.twig
    src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/fielddefinition/view/sesproductselectiontype.html.twig
    src/Siso/Bundle/ProductSelectionTypeBundle/Resources/views/fieldtypes/sesproductselectiontype.html.twig
    ```

??? "Changed templates"

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

Follow these changes only, if you are using Order history in your project.

#### Constructor changes

`src/Siso/Bundle/OrderHistoryBundle/Services/DateTimeService.php`

### Templates

??? "Changed templates"

    ```
    src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/Components/fields.html.twig
    src/Siso/Bundle/OrderHistoryBundle/Resources/views/OrderHistory/detail.html.twig
    ```

## silver.customercenter

Follow these changes only, if you are using Customer center in your project.

### Interface changes

??? "FormProcessorInterface"

    ``` 
    - public function execute(Form $form, array $lastResult = array());
    + public function execute(FormInterface $form, array $lastResult = array());
    ```

### Constructor changes

`Symfony\Bundle\FrameworkBundle\Templating\DelegatingEngine` in constructor replaced by `Symfony\Component\Templating\EngineInterface`:

```
src/Siso/Bundle/CustomerCenterBundle/EventListener/CheckBudgetEventListener.php
src/Siso/Bundle/CustomerCenterBundle/EventListener/RemoveBasketListener.php
```

### Templates

- Templates have changed in the following location (Legacy code was refactored):  
    `src/Siso/Bundle/CustomerCenterBundle/Resources/views/CustomerCenter/Emails`

### Configuration changes

- Policy prefix has changed from 'ses_' to 'siso_' according to naming schema in silver-eShop policies.
- Fix deprecated routing key (pattern -> path). An example for all changes.

??? "routing.yml"
    
    ```
    - pattern:  /customer-center
    + path:  /customer-center
    ```
    
- Fully-Qualified Class Names instead of type names:
    
??? "customercenter.yml""

    ``` 
    - type: choice
    + type: Symfony\Component\Form\Extension\Core\Type\ChoiceType


    - type: text
    + type: Symfony\Component\Form\Extension\Core\Type\TextType

    ...
    ```
    
## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above\!
