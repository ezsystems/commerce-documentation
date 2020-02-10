# Template resolver

## General

Template resolver allows to **override** templates, e.g. to override e-shop templates in project bundles. It is another way how to override templates beside the default Symfony overriding functionality (<http://symfony.com/doc/current/book/templating.html#overriding-bundle-templates>).

Moreover, a web-site redesign is an another case when template resolver could come useful.

The idea behind the template resolver came from old eZ Publish 4 times, when it was possible to override any template using [design combinations](https://doc.ez.no/eZ-Publish/Technical-manual/5.x/Concepts-and-basics/Designs/Design-combinations).

Template resolver sequentially goes through all configured bundles and designs (if specified), looking for the requested template. At last, if the requested template still hasn't been found, template resolver takes this template or default one if the requested template doesn't exist.

An override is activated by a template resolver configuration. You can configure:

  - list of bundles
  - list of designs (folders with templates)
  - default template

## How to configure and use template resolver

### Example 1: how to override templates

Let's say you have a project with a bundle src/Client/Bundle**/WebsiteBundle**, a site access **website\_de\_de** and you want to override  eZ Commerce bundle templates.

#### Step 1. Change your config\_{env}.yml or parameters.yml

Template resolver is deactivated in the default configuration and has to be activated\!

``` 
parameters:
    # 1. Enable template resolver
    siso_tools.website_de_de.template_resolver.enabled: true    # true|false
 
    # 2. Define that your bundle is able to override templates
    siso_tools.website_de_de.template_resolver.bundles: [ClientWebsiteBundle]
```

Template resolver uses dynamic configuration of [eZ Publish config resolver](https://doc.ez.no/display/EZP/Configuration#Configuration-DynamicconfigurationwiththeConfigResolver).

If you wand to have one configuration for all site accesses, change site access name to "default". E.g.:

``` 
siso_tools.default.template_resolver.enabled: false
```

#### Step 2. Create new templates in ClientWebsiteBundle

In order to override templates you have to reflect a structure of eZ Commerce vendor bundles, e.g.:  

|                   |                                                                                                                                         |
| ----------------: | --------------------------------------------------------------------------------------------------------------------------------------- |
| Original template | /vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/**Resources/views/Catalog/catalog.html.twig** |
|      New template | /src/Client/Bundle/WebsiteBundle/**Resources/views/**Catalog/catalog.html.twig****                                                      |

In this example the template from ClientWebsiteBundle overrides **catalog.html.twig** from SilversolutionsEshopBundle.

Next, you can create more and more templates using the folder structure of eZ Commerce bundle.

### Example 2: how to override templates using designs

Design is just an additional folder in your Resources/views. Templates in the design folder have to reflect a structure of the bundle or bundles that you want to override.

Assume that you are preparing templates for a website redesign with a provisional name "redesign2015".

#### Step 1. Change your config\_{env}.yml or parameters.yml

``` 
parameters:
    siso_tools.website_de_de.template_resolver.enabled: true    # true|false
    siso_tools.website_de_de.template_resolver.bundles: [ClientWebsiteBundle]
 
    # Define design name
    siso_tools.website_de_de.template_resolver.designs: [redesign2015]
```

#### Step 2. Create new templates in ClientWebsiteBundle in the design folder

Create new folder Resources/views/designs/redesign2015 in your bundle and put new templates there:

|                   |                                                                                                                                                                            |
| ----------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Original template | /vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/**Resources/views/Catalog/catalog.html.twig**                                                 |
|      New template | /src/Client/Bundle/WebsiteBundle/****Resources/views/designs/**redesign2015**/**Catalog/******catalog.html.twig**** |

In this example the template from the desing "redesign2015" in the ClientWebsiteBundle overrides **catalog.html.twig** from SilversolutionsEshopBundle.

Templates in design folders always have precedence over other templates.

If you have both ******redesign2015**/**Catalog/******catalog.html.twig**** and **********Catalog/******catalog.html.twig********, the first one will win.

## Configuration

| Parameter                                                                                | Type                           | Description                                                 | Default                                                         | Example                                                         |
| ---------------------------------------------------------------------------------------- | ------------------------------ | ----------------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------------------- |
| siso\_tools.\<scope\>.template\_resolver.**enabled**                        | bool (true|false) | Enable or disable template resolver                         | true                                                            | true                                                            |
| siso\_tools.\<scope\>.template\_resolver.**bundles**           | array                          | List of bundle names                           | \[ \] (all bundles)                                             | \[Bundle1, Bundle2, Bundle3\]                      |
| siso\_tools.\<scope\>.template\_resolver.**designs**           | array                          | List of bundle names                           | \[ \] (none)                                                    | \[Design1, Design2\]                               |
| siso\_tools.\<scope\>.template\_resolver.**default\_template** | string                         | Default template to return if nothing is found | SisoToolsBundle:TemplateResolver:default.html.twig | SisoToolsBundle:TemplateResolver:default.html.twig |

## How to develop with TemplateResolver

### Using in templates

To make a template overridable, you have to use a template filter st\_resolve\_template with its name.

``` 
{% extends "SilversolutionsEshopBundle::pagelayout.html.twig"|st_resolve_template %}
<div class="grid_4">
    {{ include('SilversolutionsEshopBundle:parts:address.html.twig'|st_resolve_template, {'class' : 'styled_list', 'address' : buyerAddress, 'displayEmail' : false, 'displayPhone' : false}) }}

```

It is not possible to include blocks with "[use](http://twig.sensiolabs.org/doc/tags/use.html)" tag because of specific implementation of this tag.

Because `use` statements are resolved independently of the context passed to the template, the template reference cannot be an expression.

### Using in PHP code

#### Using in controllers

To make using of TemplateResolver in controllers easier, you can inherit your controller from Silversolutions\\Bundle\\EshopBundle\\Controller\\BaseController. In this case *render* and *renderView* method will get rendered template using TemplateResolver, i.e. overriden template.

``` 
use Silversolutions\Bundle\EshopBundle\Controller\BaseController;
 
class ExampleController extends BaseController
{
    public function showAction(Request $request)
    {
        return $this->render(
            'SilversolutionsEshopBundle:Basket:show.html.twig',
            array(
                'param' => 'taram'
            )
        );
    }
}
```

#### General using

Alternatively, you can inject TemplateResolver in your service (or take it from container) and resolve template there

``` 
/** @var TemplateResolverServiceInterface $templateResolverService */
$templateResolverService = $this->get('siso_tools.template_resolver');

$resolvedTemplate = $templateResolverService->resolve('YourBundle:redesign2015/Catalog/catalog.html.twig');
```

## Debugging

You can information about TemplateResolver configuration and overridden templates in the SilverSolutions tab of symfony web provider.

![](attachments/23560645/23563341.png)

Example:

![](attachments/23560645/23563383.png)

## Limitations

  - pagelayout.html.twig can not be resolved via template resolver

  - it is not possible to override templates included with the twig **use** operator.

  - it is not possible to use the template resolver in the configuration, therefore for overriding standard templates for e.g. full eZ template or templates defined for eZ flow, you need to override the configuration  

    ``` 
    ezpublish:
        system:
            %siteaccess_group%:
                location_view:
                    full:
                        silverModuleRuleset:
                            #change the template name here
                            template: SilversolutionsEshopBundle::st_module.html.twig
                            match:
                                Identifier\ContentType: [st_module]
                        silverEshopProductCatalogRuleset:
                            #change the template name here
                            template: SilversolutionsEshopBundle::ses_productcatalog.html.twig
                            match:
                                Identifier\ContentType: [ses_productcatalog]
    ```

## Attachments:

![](images/icons/bullet_blue.gif) [ssl tab](attachments/23560645/23563386) (application/x-upload-data)  
![](images/icons/bullet_blue.gif) [ssl tab](attachments/23560645/23563344) (application/x-upload-data)  
![](images/icons/bullet_blue.gif) [ssl tab](attachments/23560645/23563388) (application/octet-stream)  
![](images/icons/bullet_blue.gif) [ssl tab](attachments/23560645/23563387) (application/octet-stream)  
![](images/icons/bullet_blue.gif) [ssl tab](attachments/23560645/23563389) (application/x-upload-data)  
![](images/icons/bullet_blue.gif) [download.png](attachments/23560645/23563390.png) (image/png)  
![](images/icons/bullet_blue.gif) [ssl tab](attachments/23560645/23563391) (application/octet-stream)  
![](images/icons/bullet_blue.gif) [ssl tab](attachments/23560645/23563392) (application/octet-stream)  
![](images/icons/bullet_blue.gif) [ssl tab](attachments/23560645/23563382) (application/x-upload-data)  
![](images/icons/bullet_blue.gif) [2.png](attachments/23560645/23563341.png) (image/png)  
![](images/icons/bullet_blue.gif) [tpl-resolver.png](attachments/23560645/23563383.png) (image/png)  
