#  Release notes 2.4 (4.1) 

## eZ Commerce version 2.4

  - supported eZ Platform Version V2.4  

 If you apply this eZ Commerce version in the project, always check the latest tagged version.

## Server requirements

eZ Commerce relies on eZ Platform software that is built to rely on existing technologies and standards, mainly:

  - PHP scripting language: 7  

  - SQL database: *MySql/MariaDB or PostgreSQL*

  - Web Server: Apache/Nginx

  - Java JRE  (Oracle-Sun/OpenJDK) when Solr is used (for use with eZ Find search engine)

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
<tr>
<td><div class="content-wrapper">
<p><a href="http://rm.extranet.silversolutions.de/issues/16201" class="external-link">#16201</a></p>
</td>
<td><p>No subfolder for assets</p></td>
<td>Change the default configuration for asset folders to 0<br />
</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16194" class="external-link">#16194</a></td>
<td>Toggle price button</td>
<td><p>Design improvement and a better spot</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16196" class="external-link">#16196</a></td>
<td><p>Toggle price button (Advanced version only)</p></td>
<td>Stock in product detail not hidden anymore by toggle feature</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16026" class="external-link">#16026</a></td>
<td>Contact form</td>
<td>GDPR adaptation: Add textmodule for privacy policy in contact form</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/13423" class="external-link">#13423</a></td>
<td>New Dropdownmenu Component</td>
<td>Can be used instead of Megamenu Component</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16014" class="external-link">#16014</a></td>
<td>Exchanged Cloudzoom plugin with xZoom plugin</td>
<td><p>xZoom plugin is open source</p>
<p><a href="http://confluence.extranet.silversolutions.de:8090/display/EZC14/Component+-+Image+Zoom" class="external-link">xZoom documentation</a></p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16229" class="external-link">#16229</a></td>
<td>GDPR e-mail encryption</td>
<td>Added parameter for mail encryption</td>
</tr>
<tr>
<td><p><a href="http://rm.extranet.silversolutions.de/issues/16519" class="external-link">#16519</a></p></td>
<td>Password Reminder</td>
<td>Template improvements</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16228" class="external-link">#16228</a></td>
<td>Checkout</td>
<td>Adapt GDPR textmodules and checkboxes</td>
</tr>
<tr>
<td><p><a href="http://rm.extranet.silversolutions.de/issues/16197" class="external-link">#16197</a></p></td>
<td>Forms validation</td>
<td>Make ZIP validator more flexibly usable</td>
</tr>
<tr>
<td><p><a href="http://rm.extranet.silversolutions.de/issues/16193" class="external-link">#16193</a></p></td>
<td>Log search requests</td>
<td><p>Due to the GDPR regulations the suer id will be not stored any more in search logs.</p>
<p>New parameter siso_core.default.gdpr.store_user_id_for_search: false</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16608" class="external-link">#16608</a></td>
<td><p>Checkout design</p></td>
<td>Remove grey outline in checkout steps</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16766" class="external-link">#16766</a></td>
<td>Specification fieldtype</td>
<td><p>sesspecification fieldtype supports 3 data fields</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16198" class="external-link">#16198</a></td>
<td>BasketLine API change</td>
<td><p>Typo in name of method remoteFromRemoteDataMap in class EshopBundle\Entity\BasketLine</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16140" class="external-link">#16140</a></td>
<td><p>Orderhistory</p></td>
<td><p>orderhistory_view policy check for orderhistory usage added</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/15313" class="external-link">#15313</a></td>
<td><p>Template Resolver</p></td>
<td>Template Resolver deactivated by default configuration</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16819" class="external-link">#16819</a></td>
<td>ERP default configuration (Advanced version only)</td>
<td>select_contact_mode disabled in standard<br />
</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16520" class="external-link">#16520</a></td>
<td>Template extensions</td>
<td>Support simple fields in ses_render_field</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16760" class="external-link">#16760</a></td>
<td><p>New Form syntax</p></td>
<td>Adjust form syntax for Symfony 3</td>
</tr>
<tr>
<td><p><a href="http://rm.extranet.silversolutions.de/issues/16772" class="external-link">#16772</a></p></td>
<td>Legacy</td>
<td>Removal of all eZ Publish legacy code and references</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16973" class="external-link">#16973</a></td>
<td>User configuration and fields</td>
<td>Provide separate user groups for business and private customer and add first and last name fields to eZ User content type.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17006" class="external-link">#17006</a></td>
<td>Registration</td>
<td>Private customer registration was enhanced by the possibility to send the activation email to the shop admin instead of the customer and to inform the customer via email when the shop account was activated via activation link in email.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17140" class="external-link">#17140</a></td>
<td>Basket</td>
<td>Add new indices for better performance</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17365" class="external-link">#17365</a></td>
<td>Storage</td>
<td>eZ Commerce is using "ecommerce/storage" instead of "harmony/storage" now.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17103" class="external-link">#17103</a></td>
<td>Show VAT for different customer types</td>
<td>Display VAT differently for various customer types. Affected shop parts are the basket, product list, product detail, slider, emails and bestseller.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17265" class="external-link">#17265</a></td>
<td>Improvement econtent repository</td>
<td>New helper methods to improve econtent imports</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16040" class="external-link">#16040</a></td>
<td>ecommerce cockpit optimization</td>
<td>Refactoring and enhancement of the ecommerce cockpit</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16341" class="external-link">#16341</a></td>
<td>Navigation</td>
<td>Introduce tag caching for text modules</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16199" class="external-link">#16199</a></td>
<td>Econtent</td>
<td>Specifications are stored in a JSON field type</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16290" class="external-link">#16290</a></td>
<td><p>Discontinued products</p></td>
<td>A Listener can check if a product is not sold anymore (discontinued) and the user might have to be informed that this product is not available any more. </td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16652" class="external-link">#16652</a></td>
<td>ERP administration</td>
<td>New policies added</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/15311" class="external-link">#15311</a></td>
<td>Quickorder</td>
<td>Add eContent filters to quick order autosuggest</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16632" class="external-link">#16632</a></td>
<td><p>Tooltips</p></td>
<td>Replace js tooltips with css tooltips for better performance (especially in Edge and Firefox)</td>
</tr>
</tbody>
</table>

## Bug fixes

List of bugs that were fixed in this version.

<table>
<thead>
<tr class="header">
<th>Ticket</th>
<th>Topic</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<a href="http://rm.extranet.silversolutions.de/issues/16004" class="external-link">#16004</a>
</td>
<td><p>mobile view Wishlist</p></td>
<td>Improved quantity field for tablets</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16443" class="external-link">#16443</a></td>
<td><p>Quickorder delete lines</p></td>
<td>Only add the line if the delete checkbox was not checked</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16206" class="external-link">#16206</a></td>
<td>Serializing of catalogElement</td>
<td>Issue with toHash() fromHash() method in the PriceField</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16767" class="external-link">#16767</a></td>
<td>PHP7</td>
<td>Fix PHP7 compatibility</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16194#change-138605" class="external-link">#16194</a></td>
<td><p>Price Toggle Button (Advanced version only)</p></td>
<td>Toggle bestseller prices</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16755" class="external-link">#16755</a></td>
<td>Order History</td>
<td>Fix strict errors about wrong type hinting.</td>
</tr>
<tr>
<td><p><a href="http://rm.extranet.silversolutions.de/issues/16208" class="external-link">#16208</a></p></td>
<td>Business user VAT number (Advanced version only)</td>
<td>VAT number is only required for EU countries</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16205" class="external-link">#16205</a></td>
<td>Templates</td>
<td>Remove redundant template code</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16286" class="external-link">#16286</a></td>
<td>Tabs in backend</td>
<td>Don't show eCommerce tab in non-related content types.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16866" class="external-link">#16866</a></td>
<td>eZ Backend</td>
<td>Bug in ContentModificationSlot for move subtree</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16207" class="external-link">#16207</a></td>
<td>TranslationService</td>
<td><p>- The siteaccess is almost ignored in a specific situation.   </p>
<p>- No fallback language will be used if a siteaccess is defined.</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16878" class="external-link">#16878</a></td>
<td>Delegate</td>
<td>Strict comparison for templates</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16903" class="external-link">#16903</a></td>
<td>Quick order</td>
<td>Fix session management for CSV upload</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17099" class="external-link">#17099</a></td>
<td>Assetservice</td>
<td>Default strategy set to "dataprovider" since "merge" could cause performance issues</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16962" class="external-link">#16962</a></td>
<td>Registration</td>
<td>Make registration link eZ Commerce aware</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16867" class="external-link">#16867</a></td>
<td><p>Econtent</p></td>
<td>Econtent mapping
<pre class="tw-data-text tw-ta tw-text-medium"><code>corresponds to new data model</code></pre></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16651" class="external-link">#16651</a></td>
<td><p>CustomerProfileData</p></td>
<td>profileData in EzErpCustomerProfileDataService was not updated by UpdateCustomerProfileDataProcessor</td>
</tr>
<tr>
<td><p><a href="http://rm.extranet.silversolutions.de/issues/16781" class="external-link">#16781</a></p>
<p><a href="http://rm.extranet.silversolutions.de/issues/16828" class="external-link">#16828</a></p></td>
<td>Improvements for econtent</td>
<td><p>optimised SwitchDataProviderCommand and updated to eZ Platform 2.3<br />
EcontentCatalogFactory.php <br />
  - Fixed issue with php 7.2<br />
  -  adapted mapping for float fields<br />
  - fixed issue in case no image field is provided by econtent<br />
EcontentFieldNameResolver.php – Added support for larger variant sets (solr length issue)<br />
StorageService.php – Fixed issue if finder does not find asets <br />
Econtent configuration  - added example config for catalog segmentation</p>
<p>Store variants in JSON field instead of matrix field type</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17210" class="external-link">#17210</a></td>
<td>Address management</td>
<td>Fix Exception in StoreDeliveryAddressWithoutErpConnectionListener</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17280" class="external-link">#17280</a></td>
<td>Wrong icon size in configuration settings</td>
<td>After the update to eZ Platform 2.4 the accordion icons have been too big</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/17177" class="external-link">#17177</a></td>
<td>Product catalog</td>
<td><p>Fixed issue with DefaultRouter and prefixes/default siteaccess. Caused 404 pages on product urls</p></td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16959" class="external-link">#16959</a></td>
<td>Orderhistory</td>
<td>Fixed issue for getting local order details.</td>
</tr>
<tr>
<td><a href="http://rm.extranet.silversolutions.de/issues/16410" class="external-link">#16410</a></td>
<td><p>Registration form</p></td>
<td>Remove privacy policy validation from registration form</td>
</tr>
<tr>
<td><p><a href="http://rm.extranet.silversolutions.de/issues/17115" class="external-link">#17115</a></p></td>
<td>Orderhistory</td>
<td>Fixed issue where wrong order number was displayed in the shop</td>
</tr>
</tbody>
</table>

## New plugins

no change

## Tagged versions

### eZ Commerce

no change

###   
silver.orderhistory

no change

## JavaScript libraries and plugins

no change

# API Changes

## eZ Commerce

### Database changes

#### Update doctrine

``` 
php bin/console doctrine:schema:update --force
```

### Ez backend changes

#### Content type User

The standard will from now on use the separate fields first\_name and last\_name instead of the combined field name.

Therefore the content type User has to be adapted:

  - Move fields first\_name and last\_name below the name field by changing the priority.
  - The fields first\_name and last\_name should be required ![(warning)](images/icons/emoticons/warning.png) Make sure that all existing users contain values for first\_name and last\_name\!
  - Change Content name pattern to '\<first\_name\> \<last\_name\>'.

#### User groups for Shop users

The standard supports separate user groups for private and business customers.

If you want to make use of this, create these user groups in the user group *Shop user. *The location id of the user groups has to be configured, see section Configuration for details.

![(warning)](images/icons/emoticons/warning.png) The existing Shop users have to be sorted into the new groups manually. 

### Classes

The property ***vatCountryCode*** has been removed from from model \\Silversolutions\\Bundle\\EshopBundle\\Entities\\Forms\\RegisterBusiness and it's respective service implementation \\Silversolutions\\Bundle\\EshopBundle\\Entities\\Forms\\Types\\RegisterBusinessType

See templates section for necessary changes.

###   
Configuration

**New configurations**

``` 
# config parameter to define the used admin siteaccess for links to eZ backend
siso_core.default.related_admin_site_access: 'admin'
# optional parameter to send user activation email after private customer registration to shop admin for validation instead of sendig it to the customer  
siso_core.default.user_activation_receiver: xx@yyy.com
# parameter to activate information email to customer after successful activation of shop account via activation link in email
siso_core.default.info_email_after_user_activation: true/false
```

To separate business and private user in one installation two user groups in the user group *Shop user* are used. Next to the user groups a configuration is needed:

``` 
siso_core.default.user_group_location.business: 385
siso_core.default.user_group_location.private: 388
```

**New storage path**

``` 
silver_tools.default.storagePath: ecommerce/storage
```

### Deprecated content

no change

### Templates

**Modified templates**

The templates SilversolutionsEshopBundle:Emails:password\_reminder.html.twig and SilversolutionsEshopBundle:Emails:password\_reminder.txt.twig do not get the receivers e-mail address anymore, but the full name in the template parameter contactMailReceiver.

  - **src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout\_delivery\_address.html.twig**
  - **src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout\_invoice\_address.html.twig**
  - **src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout\_login.html.twig**
  - **src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout\_shipping\_payment.html.twig**
  - **src/Silversolutions/Bundle/EshopBundle/Resources/views/Checkout/checkout\_summary.html.twig**

**Business user templates** Expand source 

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

**src/Silversolutions/Bundle/EshopBundle/Resources/views/pagelayout.html.twig** Expand source 

``` 
   'bundles/silversolutionseshop/vendor/underscore/underscore.js'
   'bundles/silversolutionseshop/vendor/backbone/backbone.js'
   'bundles/silversolutionseshop/vendor/hinclude/hinclude.js'
-  'bundles/silversolutionseshop/vendor/cloudzoom/cloudzoom.js'
+  'bundles/silversolutionseshop/vendor/xzoom/xzoom.js'
   'bundles/silversolutionseshop/vendor/sticky/jquery.sticky.js'
   'bundles/silversolutionseshop/vendor/Sortable/Sortable.min.js'
```

**src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productDetail.html.twig** Expand source 

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

**src/Silversolutions/Bundle/EshopBundle/Resources/views/Basket/show\_wishlist\_part.html.twig** Expand source 

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

  - medium breakpoint changed from "$medium-breakpoint: em-calc(<span class="idiff">1052);" to $medium-breakpoint: em-calc(<span class="idiff">850); in "src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/settings/\_foundation.scss"
  - Menu icon bar grid changed from "$icon-bar-wrapper-column-width: <span class="idiff">5 \!default;" to "$icon-bar-wrapper-column-width: <span class="idiff">6 \!default;" in "src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/storm/\_extend.components.icon-bar.scss"

### Vendor bundles + config

### Overridden vendor services

no change

## Translations

### Textmodules

New:
  - label.accept\_cancellation\_policies
  - data\_privacy\_hint  

Removed:

  - label.accept\_data\_protection\_and\_cancellation\_policies

### Symfony Translations

New:

  - subject.registration\_validation
  - subject.complete\_registration
  - link.activate\_user
  - link.edit\_user
  - label.user\_name
  - mail\_text.new\_private\_user\_registration

Topic specific (not complex) changes in eZ Commerce 

no change

## Hotfixes

To check the API changes for the hotfixes, please check the hotfix patches above\!
