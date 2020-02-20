# Release notes 2.4 (4.1)

## eZ Commerce version 2.4

- supported eZ Platform v2.4  

If you apply this eZ Commerce version in the project, always check the latest tagged version.

## Server requirements

eZ Commerce relies on eZ Platform software that is built to rely on existing technologies and standards, mainly:

- PHP scripting language: 7
- SQL database: *MySql/MariaDB or PostgreSQL*
- Web Server: Apache/Nginx
- Java JRE  (Oracle-Sun/OpenJDK) when Solr is used (for use with eZ Find search engine)

## New features and improvements

List of new features and improvements.

|Ticket|Topic|Description|
|--- |--- |--- |
|#16201|No subfolder for assets|Change the default configuration for asset folders to 0|
|#16194|Toggle price button|Design improvement and a better spot|
|#16196|Toggle price button (Advanced version only)|Stock in product detail not hidden anymore by toggle feature|
|#16026|Contact form|GDPR adaptation: Add textmodule for privacy policy in contact form|
|#13423|New Dropdownmenu Component|Can be used instead of Megamenu Component|
|#16014|Exchanged Cloudzoom plugin with xZoom plugin|xZoom plugin is open source</br>[xZoom documentation](../cookbook/frontend_customizing/4_front_end_stack_in_details/4.2_customized_foundation_framework/4.2.2_components/component_image_zoom.md)|
|#16229|GDPR e-mail encryption|Added parameter for mail encryption|
|#16519|Password Reminder|Template improvements|
|#16228|Checkout|Adapt GDPR textmodules and checkboxes|
|#16197|Forms validation|Make ZIP validator more flexibly usable|
|#16193|Log search requests|Due to the GDPR regulations the user id will be not stored any more in search logs.</br>New parameter siso_core.default.gdpr.store_user_id_for_search: false|
|#16608|Checkout design|Remove grey outline in checkout steps|
|#16766|Specification fieldtype|sesspecification fieldtype supports 3 data fields|
|#16198|BasketLine API change|Typo in name of method remoteFromRemoteDataMap in class EshopBundle\Entity\BasketLine|
|#16140|Orderhistory|orderhistory_view policy check for orderhistory usage added|
|#15313|Template Resolver|Template Resolver deactivated by default configuration|
|#16819|ERP default configuration (Advanced version only)|select_contact_mode disabled in standard|
|#16520|Template extensions|Support simple fields in ses_render_field|
|#16760|New Form syntax|Adjust form syntax for Symfony 3|
|#16772|Legacy|Removal of all eZ Publish legacy code and references|
|#16973|User configuration and fields|Provide separate user groups for business and private customer and add first and last name fields to eZ User content type.|
|#17006|Registration|Private customer registration was enhanced by the possibility to send the activation email to the shop admin instead of the customer and to inform the customer via email when the shop account was activated via activation link in email.|
|#17140|Basket|Add new indices for better performance|
|#17365|Storage|eZ Commerce is using "ecommerce/storage" instead of "harmony/storage" now.|
|#17103|Show VAT for different customer types|Display VAT differently for various customer types. Affected shop parts are the basket, product list, product detail, slider, emails and bestseller.|
|#17265|Improvement econtent repository|New helper methods to improve econtent imports|
|#16040|ecommerce cockpit optimization|Refactoring and enhancement of the ecommerce cockpit|
|#16341|Navigation|Introduce tag caching for text modules|
|#16199|Econtent|Specifications are stored in a JSON field type|
|#16290|Discontinued products|A Listener can check if a product is not sold anymore (discontinued) and the user might have to be informed that this product is not available any more.|
|#16652|ERP administration|New policies added|
|#15311|Quickorder|Add eContent filters to quick order autosuggest|
|#16632|Tooltips|Replace js tooltips with css tooltips for better performance (especially in Edge and Firefox)|

## Bug fixes

List of bugs that were fixed in this version.

|Ticket|Topic|Description|
|--- |--- |--- |
|#16004|mobile view Wishlist|Improved quantity field for tablets|
|#16443|Quickorder delete lines|Only add the line if the delete checkbox was not checked|
|#16206|Serializing of catalogElement|Issue with toHash() fromHash() method in the PriceField|
|#16767|PHP7|Fix PHP7 compatibility|
|#16194|Price Toggle Button (Advanced version only)|Toggle bestseller prices|
|#16755|Order History|Fix strict errors about wrong type hinting.|
|#16208|Business user VAT number (Advanced version only)|VAT number is only required for EU countries|
|#16205|Templates|Remove redundant template code|
|#16286|Tabs in backend|Don't show eCommerce tab in non-related content types.|
|#16866|eZ Backend|Bug in ContentModificationSlot for move subtree|
|#16207|TranslationService|- The siteaccess is almost ignored in a specific situation.</br>- No fallback language will be used if a siteaccess is defined.|
|#16878|Delegate|Strict comparison for templates|
|#16903|Quick order|Fix session management for CSV upload|
|#17099|Assetservice|Default strategy set to "dataprovider" since "merge" could cause performance issues|
|#16962|Registration|Make registration link eZ Commerce aware|
|#16867|Econtent|Econtent mapping corresponds to new data model|
|#16651|CustomerProfileData|profileData in EzErpCustomerProfileDataService was not updated by UpdateCustomerProfileDataProcessor|
|#16781</br>#16828|Improvements for econtent|optimised SwitchDataProviderCommand and updated to eZ Platform 2.3</br>EcontentCatalogFactory.php</br>- Fixed issue with php 7.2</br>-  adapted mapping for float fields</br>- fixed issue in case no image field is provided by econtent</br>EcontentFieldNameResolver.php – Added support for larger variant sets (solr length issue)</br>StorageService.php – Fixed issue if finder does not find asets</br>Econtent configuration  - added example config for catalog segmentation</br>Store variants in JSON field instead of matrix field type|
|#17210|Address management|Fix Exception in StoreDeliveryAddressWithoutErpConnectionListener|
|#17280|Wrong icon size in configuration settings|After the update to eZ Platform 2.4 the accordion icons have been too big|
|#17177|Product catalog|Fixed issue with DefaultRouter and prefixes/default siteaccess. Caused 404 pages on product urls|
|#16959|Orderhistory|Fixed issue for getting local order details.|
|#16410|Registration form|Remove privacy policy validation from registration form|
|#17115|Orderhistory|Fixed issue where wrong order number was displayed in the shop|

## New plugins

no change

## Tagged versions

### eZ Commerce

no change

### silver.orderhistory

no change

## JavaScript libraries and plugins

no change

# API Changes

## eZ Commerce

### Database changes

#### Update doctrine

``` bash
php bin/console doctrine:schema:update --force
```

### eZ backend changes

#### Content type User

The standard will from now on use the separate fields first_name and last_name instead of the combined field name.

Therefore the content type User has to be adapted:

  - Move fields `first_name` and `last_name` below the name field by changing the priority.
  - The fields `first_name` and `last_name` should be required. Make sure that all existing users contain values for `first_name` and `last_name`!
  - Change Content name pattern to `<first_name> <last_name>`.

#### User groups for Shop users

The standard supports separate user groups for private and business customers.

If you want to make use of this, create these user groups in the user group *Shop user*. The location id of the user groups has to be configured, see section Configuration for details.

The existing Shop users have to be sorted into the new groups manually. 

### Classes

The property ***vatCountryCode*** has been removed from model `\Silversolutions\Bundle\EshopBundle\Entities\Forms\RegisterBusiness` and its respective service implementation `\Silversolutions\Bundle\EshopBundle\Entities\Forms\Types\RegisterBusinessType`

See templates section for necessary changes.

### Configuration

**New configurations**

``` yaml
# config parameter to define the used admin siteaccess for links to eZ backend
siso_core.default.related_admin_site_access: 'admin'
# optional parameter to send user activation email after private customer registration to shop admin for validation instead of sending it to the customer  
siso_core.default.user_activation_receiver: xx@yyy.com
# parameter to activate information email to customer after successful activation of shop account via activation link in email
siso_core.default.info_email_after_user_activation: true/false
```

To separate business and private user in one installation two user groups in the user group *Shop user* are used. Next to the user groups a configuration is needed:

``` yaml
siso_core.default.user_group_location.business: 385
siso_core.default.user_group_location.private: 388
```

**New storage path**

``` yaml
silver_tools.default.storagePath: ecommerce/storage
```

### Deprecated content

no change

### Templates

**Modified templates**

The templates `SilversolutionsEshopBundle:Emails:password_reminder.html.twig` and `SilversolutionsEshopBundle:Emails:password_reminder.txt.twig` do not get the receivers e-mail address anymore, but the full name in the template parameter `contactMailReceiver`.

- `src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_delivery_address.html.twig`
- `src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_invoice_address.html.twig`
- `src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_login.html.twig`
- `src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_shipping_payment.html.twig`
- `src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout_summary.html.twig`

??? "Business user templates"

    ``` 
    --- a/src/Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_RegisterBusiness.html.twig
    +++ b/src/Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_RegisterBusiness.html.twig
    @@ -13,7 +13,7 @@
         </tr>
         <tr>
             <td>
    -            {{ 'VAT country code'|st_translate }}: {{ form.vatCountryCode }}<br/>
    +            {# Combination between VAT Number and VAT Country Code #}
                 {{ 'VAT number'|st_translate }}: {{ form.vatNumber }}<br/>
                 {% if form.trader %}
                     {{ 'Customer is a trader. Here are the data:'|st_translate }}<br/><br/>
    --- a/src/Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_RegisterBusiness.txt.twig
    +++ b/src/Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_RegisterBusiness.txt.twig
    @@ -1,8 +1,7 @@
     {{ 'Dear Admin'|st_translate }}!
     {{ 'New business customer has registered. Please check these data:'|st_translate }}
     
    -
    -{{ 'VAT country code'|st_translate() }}:  {{ form.vatCountryCode }}
    +{# Combination between VAT Number and VAT Country Code #}
     {{ 'VAT number'|st_translate }}:  {{ form.vatNumber }}
     {{ 'Trader'|st_translate }}:  {{ form.trader }}
     {{ 'Commercial register number'|st_translate }}:  {{ form.tradeRegisterNumber }}
    --- a/src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/register_business.html.twig
    +++ b/src/Silversolutions/Bundle/EshopBundle/Resources/views/Forms/register_business.html.twig
    @@ -49,13 +49,6 @@
                         <small class="error">{{ error.message }}</small>
                     {% endfor %}
                 
    -            <div{% if form.vatCountryCode.vars.errors is not empty %} class="error"{% endif %}>
    -                {{ form_label(form.vatCountryCode)}}
    -                {{ form_widget(form.vatCountryCode) }}
    -                {% for error in form.vatCountryCode.vars.errors %}
    -                    <small class="error">{{ error.message }}</small>
    -                {% endfor %}
    -            
                 <div{% if form.vatNumber.vars.errors is not empty %} class="error"{% endif %}>
                     {{ form_label(form.vatNumber)}}
                     {{ form_widget(form.vatNumber) }}

    ```

??? "src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig""

    ``` 
       'bundles/silversolutionseshop/vendor/underscore/underscore.js'
       'bundles/silversolutionseshop/vendor/backbone/backbone.js'
       'bundles/silversolutionseshop/vendor/hinclude/hinclude.js'
    -  'bundles/silversolutionseshop/vendor/cloudzoom/cloudzoom.js'
    +  'bundles/silversolutionseshop/vendor/xzoom/xzoom.js'
       'bundles/silversolutionseshop/vendor/sticky/jquery.sticky.js'
       'bundles/silversolutionseshop/vendor/Sortable/Sortable.min.js'
    ```

??? "src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productDetail.html.twig"

    ``` 
    <div class="row js-variant-content-wrapper">
         <div class="medium-12 large-6 columns print-product-detail__img">
    -        {# TODO: how to handle video link #}
    -        {% if ses_render_field(catalogElement, 'video') %}
    -            <a class="video_link" href="{{ ses_render_field(catalogElement, 'video') }}">
    -                <span class="sprite sprite-032b-video-30 trans">
    -            </a>
    +      {# TODO: how to handle video link #}
    +      {% if ses_render_field(catalogElement, 'video') %}
    +          <a class="video_link" href="{{ ses_render_field(catalogElement, 'video') }}">
    +              <span class="sprite sprite-032b-video-30 trans">
    +          </a>
    +      {% endif %}
    +
    +      {% if orderableVariantProduct|default is not empty %}
    +        {% set productId = catalogElement.sku ~ orderableVariantProduct.variantCode %}
    +        {% set mainImage = ses_assets_main_image(orderableVariantProduct, productId)  %}
    +      {% endif %}
    +      {% if mainImage is empty %}
    +          {% set mainImage = ses_assets_main_image(catalogElement)  %}
    +      {% endif %}
    +      <img class="xzoom" src="/{{ st_imageconverter(mainImage, 'thumb_big') }}" xoriginal="/{{ st_imageconverter(mainImage, 'image_zoom') }}" />
    +
    +      <div class="thumbs u-margin-top-1x js-slick-init xzoom-thumbs" data-slick='{"slidesToShow": 4, "slidesToScroll": 4}'>
    +
    +        {% set mainImages = [mainImage] %}
    +        {% if orderableVariantProduct|default is not empty %}
    +            {% set productId = catalogElement.sku ~ orderableVariantProduct.variantCode %}
    +            {% set imageListWithoutMain = ses_assets_image_list(orderableVariantProduct, productId) %}
             {% endif %}
    -        <figure class="text-center u-no-margin u-padding-1x u-border">
    -            {% set mainImage = '' %}
    -            {% set productId = '' %}
    -            {% if orderableVariantProduct|default is not empty %}
    -                {% set productId = catalogElement.sku ~ orderableVariantProduct.variantCode %}
    -                {% set mainImage = ses_assets_main_image(orderableVariantProduct, productId)  %}
    -            {% endif %}
    -            {% if mainImage is empty %}
    -                {% set mainImage = ses_assets_main_image(catalogElement)  %}
    -            {% endif %}
    -            <a href="/{{ st_imageconverter(mainImage, 'image_zoom') }}">
    -                <img src="/{{ st_imageconverter(mainImage, 'thumb_big') }}"
    -                     alt="{% if mainImage.alternativeText is defined and mainImage.alternativeText is not empty %}{{ mainImage.alternativeText }}{% else %}{{ 'label.image_not_available'|st_translate }}{% endif %}"
    -
    -                     class="cloudzoom" id="zoom_1" data-cloudzoom="zoomSizeMode: 'image', autoInside: 640"
    -                />
    -            </a>
    -            <figcaption class="text-left u-ellipsis">{{ catalogElement.name }}</figcaption>
    -        </figure>
    -
    -                  <div class="u-margin-top-1x js-slick-init" data-slick='{"slidesToShow": 4, "slidesToScroll": 4}'>
    -                      {% set mainImages = [mainImage] %}
    -                      {% if orderableVariantProduct|default is not empty %}
    -                          {% set productId = catalogElement.sku ~ orderableVariantProduct.variantCode %}
    -                          {% set imageListWithoutMain = ses_assets_image_list(orderableVariantProduct, productId) %}
    -                      {% endif %}
    -
    -                      {% if imageListWithoutMain is empty %}
    -                          {% set imageListWithoutMain = ses_assets_image_list(catalogElement, productId) %}
    -                      {% endif %}
    -
    -                      {% set imageList = mainImages|merge(imageListWithoutMain) %}
    -                      {% if imageList|length > 0 %}
    -                          {% for item in imageList %}
    -                              <a href="#" class="cloudzoom-gallery" data-cloudzoom="useZoom: '#zoom_1', image: '/{{ st_imageconverter(item, 'thumb_big') }}', zoomImage: '/{{ st_imageconverter(item, 'image_zoom') }}'">
    -                                  <img src="/{{ st_imageconverter(item, 'thumb_smaller') }}"
    -                                       alt="{% if item.alternativeText is defined and item.alternativeText is not empty %}{{ item.alternativeText }}{% else %}{{ 'label.image_not_available'|st_translate }}{% endif %}"
    -                                  />
    -                              </a>
    -                          {% endfor %}
    -                      {% endif %}
    -                  
    -            
    +        {% if imageListWithoutMain is empty %}
    +            {% set imageListWithoutMain = ses_assets_image_list(catalogElement, productId) %}
    +        {% endif %}
    +
    +        {% set imageList = mainImages|merge(imageListWithoutMain) %}
    +        {% if imageList|length > 0 %}
    +              {% for item in imageList %}
    +
    +              <a href="/{{ st_imageconverter(item, 'image_zoom') }}">
    +                <img class="xzoom-gallery" src="/{{ st_imageconverter(item, 'thumb_smaller') }}"  xpreview="/{{ st_imageconverter(item, 'thumb_big') }}">
    +              </a>
    +
    +              {% endfor %}
    +          {% endif %}
    +        
    +      
         <div class="medium-12 large-6 columns print-product-detail__data">
             {{ include('SilversolutionsEshopBundle:Catalog:parts/productData.html.twig'|st_resolve_template) }}
             {% block product_variant %}{% endblock %}
    ```

??? "src/Silversolutions/Bundle/EshopBundle/Resources/views/Basket/show_wishlist_part.html.twig"

    ``` 
                   <div class="small-12 large-1 columns amount text-right">
                     <div class="row">
                       <div
    -                          class="small-6 columns text-left hide-for-large-up">
    +                          class="small-6 medium-10 columns text-left hide-for-large-up">
                         <strong for="quantity-{{ loop.index }}">{{ 'Quantity'|st_translate }}:</strong>
                       
    -                  <div class="small-6 large-12 columns">
    +                  <div class="small-6 medium-2 large-12 columns">
                         {% if product.minOrderQuantity|default is not empty and product.minOrderQuantity != 0 %}
                           {% set minQuantity = product.minOrderQuantity %}
                         {% else %}
    ```

### JS changes

### Responsive changes

Breakpoints are changed in this version to have a better experience on all the different devices

- medium breakpoint changed from "$medium-breakpoint: em-calc(1052);" to $medium-breakpoint: em-calc(850); in `src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/settings/_foundation.scss`
- Menu icon bar grid changed from "$icon-bar-wrapper-column-width: 5 \!default;" to "$icon-bar-wrapper-column-width: 6 \!default;" in `src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/storm/_extend.components.icon-bar.scss`

### Vendor bundles + config

### Overridden vendor services

no change

## Translations

### Textmodules

New:

- `label.accept_cancellation_policies`
- `data_privacy_hint`  

Removed:

- `label.accept_data_protection_and_cancellation_policies`

### Symfony Translations

New:

- `subject.registration_validation`
- `subject.complete_registration`
- `link.activate_user`
- `link.edit_user`
- `label.user_name`
- `mail_text.new_private_user_registration`

## Topic-specific (not complex) changes in eZ Commerce 

no change

## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above!
