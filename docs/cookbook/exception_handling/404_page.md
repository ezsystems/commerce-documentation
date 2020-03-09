# 404 Page

## Introduction

We have a configurable 404 page design. Since it's using a translatable text module we are flexible and able to use an online editor to place images, format text, etc.

By default we display an image and our standard text but it can be easily change for every project. Just edit the text module and use what's best for your project. Keep in mind to change it in every language.

## Text module location

Text module is located under Components / Textmodules / Misc

![](../img/exception_handling_3.png)

## Used Twig template

The following template is loaded and will display the text from the textmodule `404_page`.

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/Exception/exception.html.twig
```
