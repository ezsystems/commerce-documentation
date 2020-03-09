# silver.module

There  is a mechanism to load specific controllers by using eZ content object. The eZ content type is "silver.module" to define a controller path and optionally parameters for this controller. This provides a convenient opportunity to define own, simple and translatable URLs using a silver.module name to any controller.

Following attributes are possible:

|Attribute name|Description|Example value|
|--- |--- |--- |
|Name|The name of the silver.module. It is shown on the front end and is translatable.|My test module (results in an URL like: /my-test-module)|
|Controller|The controller, which should be loaded when clicking on that module. Please define the controller including the fully qualified namespace and the controller method separated by two colons (::).</br>The silver.module controller handler, whichs loads the target controller automatically validates, if the controller class and action method is available and throws an exception otherwise.|\Silversolutions\Bundle\EshopBundle\Controller\CommonController::testAction|
|Parameters|Optional parameters (list of hashes) which is directed to the target controller. Keep in mind, that only string key-value pairs are possible. The key value pairs are separated by a ";"|Parameter key: `formTypeResolver`</br>Parameter value: `contact`|

## Functionality

Using this [eZ functionality](https://doc.ezplatform.com/en/latest/guide/controllers/#enriching-viewcontroller-with-a-custom-controller), in the silver.eshop.yml a full view controller for eZ content type identifier `st_module` is defined: **SilversolutionsEshopBundle:SilverModule:viewModuleLocation()**

Within this controller **src/Silversolutions/Bundle/EshopBundle/Controller/SilverModuleController.php** a method `loadControllerAction()is called``. `This method validates the controller value defined in the silver.module content (is controller class available, is controller action available) and loads the target controller action passing current request and optional controller parameters to the target.

``` yaml
system:
    ezdemo_site_clean_group:
        location_view:
            full:
                silverModuleRuleset:
                    controller: SilversolutionsEshopBundle:SilverModule:viewModuleLocation
                    match:
                        Identifier\ContentType: [st_module]
```

!!! note

    The silver.module content object is passed to the loaded controller together with the optional parameters.
