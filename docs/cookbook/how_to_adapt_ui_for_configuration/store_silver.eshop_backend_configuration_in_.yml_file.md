#  Store silver.eShop backend configuration in .yml file 

## Store the configuration from the backend into a YML file

New implementation of a *Workflow* that stores the shop configuration from backend into a YML file in: *Resources/configuration/main*

![](attachments/23560984/23563665.png)

When the object configuration is published, then we hook into it and create a YML file with the content inside the EzObject.

Eg: object of the content type "configuration" call *"Configuration"*

**"Configuration File" field** Expand source 

``` 
siso_core.default.recaptcha:
siso_core_contact_type: '0'
siso_core_cancellation_type: '0'
silver_form_type_private: '0'
silver_form_type_business: '0'
silver_form_type_activate_business: '0'
siso_core.default.recaptcha_options.theme: light
siso_core.default.recaptcha_options.type: image
siso_core.default.recaptcha_options.size: normal
siso_core.default.recaptcha_options.defer: true
siso_core.default.recaptcha_options.async: true
siso_core.default.display_bestsellers_on_category_pages: true
siso_core.default.bestseller_limit_on_bestseller_page: 6
siso_core.default.bestseller_limit_on_catalog_page: 6
siso_core.default.bestseller_limit_in_silver_module: 6
siso_core.default.bestseller_threshold: 1
....
```

This EzObject is created/updated automatically, when the admin changes the configuration in the Ecommerce tab:

![](attachments/23560984/23563664.png)

The path where the yml files are stored is configured in the  ses\_parameters.yml

**src/Silversolutions/Bundle/EshopBundle/Resources/config/ses\_parameters.yml**

``` 
#Attention!!! These parameters are not ment to be changed! They should be only used as readonly parameters!!!
siso_core.default.silver_eshop_config_directory: '%kernel.root_dir%/Resources/silverEshopConfig/'
siso_core.default.silver_eshop_main_config_directory: 'main/'
```

There is a new service that makes the mapping of the configuration

``` 
<parameter key="siso_core.configuration_mapper.class">Silversolutions\Bundle\EshopBundle\Service\ConfigurationDocumentMapper</parameter>
... 

<service id="siso_adminuser.user_document_mapper_plugin" class="%siso_core.configuration_mapper.class%">
    <tag name="ezpublish.search.solr.document_mapper_plugin" />
    <call method="setServices">
        <argument type="service" id="ezpublish.api.repository" />
        <argument type="service" id="ezpublish.config.resolver" />
    </call>
</service>
```

And a new class "ConfigurationDocumentMapper" that implements "DocumentMapperPluginInterface"

**silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Service/ConfigurationDocumentMapper.php** Expand source 

``` 
class ConfigurationDocumentMapper implements DocumentMapperPluginInterface
{

   ....

**
 * This method generates a yml file with the content inside the configuration EzObject from the Back-end
 * - Catch the moment where the object is published in the Back-End and start with the process
 * - The YML file created has permissions 0777
 *
 * @param Content $content
 * @param Type $type
 * @param string $languageCode
 * @return Field[]
 */
public function createExtensionFields(Content $content, Type $type, $languageCode)
{
   ...
}

/**
 * Create the YML file in the file system
 *
 * @param $fileName
 * @param $contentData
 * @return void
 *
 */
protected function createYMLFile ($fileName, $contentData)
{
   ...
}
}
```

## Attachments:

![](images/icons/bullet_blue.gif) [multishopConfiguration\_1.png](attachments/23560984/23563665.png) (image/png)  
![](images/icons/bullet_blue.gif) [backEnd.png](attachments/23560984/23563664.png) (image/png)  
