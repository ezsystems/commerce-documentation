#  silver.module 

There  is a mechanism to load specific controllers by using eZ content object. The eZ content type is "silver.module" to define a controller path and optionally parameters for this controller. This provides a convenient opportunity to define own, simple and translatable URLs using a silver.module name to any controller.

Following attributes are possible:

<table>
<thead>
<tr class="header">
<th>Attribute name</th>
<th>Description</th>
<th>Example value</th>
</tr>
</thead>
<tbody>
<tr>
<td>Name</td>
<td>The name of the silver.module. It is shown on the front end and is translatable.</td>
<td>My test module (results in an URL like: /my-test-module)</td>
</tr>
<tr>
<td>Controller</td>
<td><p>The controller, which should be loaded when clicking on that module. Please define the controller including the fully qualified namespace and the controller method separated by two colons (<code>::</code>).</p>
<p>The silver.module controller handler, whichs loads the target controller automatically validates, if the controller class and action method is available and throws an exception otherwise.</p></td>
<td><code>\Silversolutions\Bundle\EshopBundle\Controller\CommonController::testAction</code></td>
</tr>
<tr>
<td>Parameters</td>
<td>An optional matrix of key-value parameters (list of hashes) which is directed to the target controller. Keep in mind, that only string key-value pairs are possible.</td>
<td>
<table>
<thead>
<tr class="header">
<th>Parameter key</th>
<th>Parameter value</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>firstString</code></td>
<td><code>Hello</code></td>
</tr>
<tr>
<td><code>secondString</code></td>
<td><code>World</code></td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>

# Functionality

Using this [eZ functionality](https://doc.ez.no/display/TECHDOC/How+to+use+a+custom+controller+to+display+a+content+item+or+location#Howtouseacustomcontrollertodisplayacontentitemorlocation-Matchingcustomcontrollers), in the silver.eshop.yml a full view controller for eZ content type identifier st\_module is defined: **SilversolutionsEshopBundle:SilverModule:viewModuleLocation()**

Within this controller **src/Silversolutions/Bundle/EshopBundle/Controller/SilverModuleController.php** a method `loadControllerAction()is called``. `This method validates the controller value defined in the silver.module content (is controller class available, is controller action available) and loads the target controller action passing current request and optional controller parameters to the target.

``` 
system:
    ezdemo_site_clean_group:
        location_view:
            full:
                silverModuleRuleset:
                    controller: SilversolutionsEshopBundle:SilverModule:viewModuleLocation
                    match:
                        Identifier\ContentType: [st_module]
```
