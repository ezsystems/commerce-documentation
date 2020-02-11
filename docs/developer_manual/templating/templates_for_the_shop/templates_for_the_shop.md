#  Templates for the shop 

In eZ Commerce templates define the structure of page layout. Since eZ Commerce is based on eZ Platform which is built with Symfony framework we are using Twig as our templating language. We combine Twig logic with semantic HTML5, and as on output rendered in the browser we get HTML5 website.

In order to find out more about Twig please visit [Twig's official documentation page](http://twig.sensiolabs.org/).

# Template headers

We decided to use a special comment block at the top of every template. Inside that block should follow a short description as well as any dependencies and any other useful information. Using these little code block we can easily parse list of all templates with their description which makes the documentation a lot easier and faster.

Here's the example block. 

``` 
{#
/**
* @file Generates general layout for the system
* @param product
* @usedby CMS
* @module content
*/
#}
```

Adding comment blocks in templates is work in progress. Currently most of the templates miss this feature.

# Template paths

Templates are stored in multiple bundles. Here is the list.

## silver.customercenter bundle

``` 
vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views
```

## silver.e-shop bundle

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views
```

## silver.orderhistory bundle

``` 
vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views
```

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-04-23 um 09.12.27.png](attachments/23560304/23563510.png) (image/png)  
