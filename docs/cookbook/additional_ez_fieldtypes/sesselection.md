#  SesSelection 

# General

This datatype is used to render a selection in the backend, so the editor can choose a value. 

Unlike the standard selection field type provided by eZ Platform, the SesSelection field type configuration is a located in an own yml file (Take a look at the example below).

For that reason, it is possible to set up siteaccess specific selection field types. This is not possible with the standard selection field type.

# Usage

## Configuration

You have to define per attribute a configuration like below. This example is for an attribute ses\_vat\_code.

**Example YML configuration**

``` 
siso_core.default.sesselection.ses_vat_code:
    default: VATNORMAL
    translation_context: ses_vat_code
    options:
        VATREDUCED: 7
        VATNORMAL: 19
```

The translation context from the configuration is optional and will be used as context for translations. 

## Change content type

Add an attribute with field type sesselection to your content type.

![](attachments/23560397/23571024.png)

The identifier of the field type (in this example. ses\_vat\_code) must be filled in a proper way as defined in the configuration above.

# Change the object

When your field type is set up in a proper way, you can assign a value in your object:

![](attachments/23560397/23571027.png)

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-12-01 um 14.23.46.png](attachments/23560397/23563484.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-12-01 um 14.23.30.png](attachments/23560397/23563475.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-12-01 um 14.24.27.png](attachments/23560397/23563474.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-23\_14-11-58.png](attachments/23560397/23571024.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-23\_14-20-43.png](attachments/23560397/23571027.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2019-02-13 um 15.18.19.png](attachments/23560397/23565271.png) (image/png)  
