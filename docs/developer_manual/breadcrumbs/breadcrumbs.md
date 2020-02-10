#  Breadcrumbs 

## Introduction

eZ Commerce supports generation of breadcrumbs throughout the shop. The customer can currently be in many different parts of the shop, but still there will be at least one generator responsible for creating breadcrumbs. 

![](attachments/23560964/23563569.png)

The breadcrumb will be generated using a general template called from the pagelayout. There is no need to define html code in full templates:

**pagelayout.html.twig**

``` 
{% block breadcrumb %}
        <div class="crumb-wrap hide-for-print">
          <div class="row">
            <div class="columns">
              {% render(controller('SilversolutionsEshopBundle:Breadcrumbs:renderBreadcrumbs')) %}

 {% endblock %} 
```

It uses schema.org markup (<http://schema.org/BreadcrumbList>) for generating the html tags. 

The breadcrumb system is using the [WhiteOctober breadcrumbs bundle](https://github.com/whiteoctober/BreadcrumbsBundle).

## Features

eZ Commerce breadcrumbs supports:

  - creation of breadcrumbs for catalog elements (e.g. products, categories)
  - creation of breadcrumbs for internal shop routes (e.g. my profile, search)
  - creation of breadcrumbs for eZ Platform content
  - creation of breadcrumbs for special eZ Commerce forms (e.g. contact form)
  - different data providers as products may come from different sources
  - generation of breadcrumbs for multiple product catalogs
  - storage of additional data in translationParameters
  - configuration for eZ content fields that should be used as breadcrumb labels

## Configuration

You can configure the eZ fields, that should be used as labels for the (eZ content) breadcrumb nodes.

The first match wins\!

``` 
parameters:
    siso_core.default.breadcrumb_content_label_fields: ['name', 'title']
```

## Important terms used in this document

Dataprovider

**Dataprovider** is the instance that provides the catalog data. eZ Commerce (Advanced version only) provides a flexible way to handle products and categories. Catalog data can be stored in different source in the shop (eZ, DB, solr) and the main goal of the data provider is to fetch the product data from the source and build the catalog elements.

Currently there are two concrete implementations for dataproviders:

<table>
<tbody>
<tr>
<td><p>Catalog Data CMS eZ</p>
<p>(Ez5CatalogDataProvider)</p></td>
<td><p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" />  products can be edited in the CMS</p>
<p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" />  good for up to 20.000 products</p>
<p><img src="images/icons/emoticons/forbidden.png" class="emoticon emoticon-minus" alt="(minus)" /> import of products costs time</p></td>
<td><p>customer has no PIM system</p>
<p>and the amount of roducts</p></td>
</tr>
<tr>
<td><p>Catalog Data eContent</p>
<p>(EcontentCatalogDataProvider)</p></td>
<td><p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /> allws more than 1 million products</p>
<p><img src="images/icons/emoticons/check.png" class="emoticon emoticon-tick" alt="(tick)" /> fast imports</p>
<p><img src="images/icons/emoticons/forbidden.png" class="emoticon emoticon-minus" alt="(minus)" /> currently no edit interface in the backend of the CMS</p></td>
<td><p>The customer is using a PIM system</p>
<p>or the ERP provides all the product information</p></td>
</tr>
</tbody>
</table>

## Before you start 

Please keep in mind that Breadcrumbs is really connected with a lot of different modules in our shop. Be sure to check these out:

  - [Catalog Element](23560458.html)
  - [Dataprovider](http://confluence.extranet.silversolutions.de:8090/display/EX/Products+from+different+sources+-+Catalog+data+providers)
  - [Navigation](Navigation_23560821.html)

**Table of contents:**

  - [Introduction](#Breadcrumbs-Introduction)
  - [Features](#Breadcrumbs-Features)
  - [Configuration](#Breadcrumbs-Configuration)
  - [Important terms used in this document](#Breadcrumbs-Importanttermsusedinthisdocument)
  - [Before you start ](#Breadcrumbs-Beforeyoustart)
  - [FAQ](#Breadcrumbs-FAQ)
  - [Templating](#Breadcrumbs-Templating)
  - [Cookbook](#Breadcrumbs-Cookbook)
  - [API](#Breadcrumbs-API)

-----

**Child Pages:**

  - [Breadcrumbs - API](Breadcrumbs---API_23560525.html)
  - [Breadcrumbs - Cookbook](Breadcrumbs---Cookbook_23560522.html)
  - [Breadcrumbs - Templates](Breadcrumbs---Templates_23560532.html)
  - [Breadcrumbs - FAQ](Breadcrumbs---FAQ_23560760.html)

## FAQ

Why do the breadcrumbs for my controller only display the root element?

Please check the routing.yml.

The RoutesBreadcrumbsGenerator need at least the parameter "**breadcrumb\_path**". This parameter usually contains the key of the current routing definition.

``` 
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index
        breadcrumb_names: Blog List
```

Some or all elements of the breadcrumbs display strange words like some\_route\_name|breadcrumb?

Please check the routing.yml.

Either no breadcrumb\_names is defined, like:

``` 
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index
```

Or the number of elements in breadcrumb\_names and breadcrumb\_path differ, like:

``` 
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index/new
        breadcrumb_names: Blog List
```

In these cases, the fallback implementation tries to translate the respective breadcrumb\_path elements with context "breadcrumb", which most likely are not translated. The best solution is to have proper path and names configuration and existing translations for the breadcrumb\_names' elements:

``` 
custom_blog_index:
    pattern:  /blog/index
    defaults:
        _controller: AppBundle:Blog:index
        breadcrumb_path: custom_blog_index/new
        breadcrumb_names: Blog List/New blog post
```

TODO This example represents the status quo, but does not respect the  [translations standards](#).

Why are no breadcrumbs displayed at all?

TODO Better description

Possibilities:

  - The 'breadcrumb' block of the pagelayout.html.twig template was overridden by the currently displayed, extending template with emtpy content.
  - The matched generator encountered an error and didn't render the breadcrumbs
  - Very unlikely but not impossible: No generator matched at all. But in the standard setup, the lowest prio RoutesBreadcrumbsGenerator checks the active Router service to match the active Request service. That's SHOULD be always the case.

How can breadcrumbs be limited to their ez content in sub shops?

The parameter 'content.tree\_root.location\_id 'is used to limit the sub shops to their ez content ( contains Node ID of the desired catalog).

If 'content.tree\_root.location\_id'is set, a criterion is used in the CatalogHelper.php to fetch the correct product catalog instead of the default one.

``` 
if ($this->configResolver->hasParameter('content.tree_root.location_id')) {
                $ezLocationRootId = $this->configResolver->getParameter('content.tree_root.location_id');
                $ezLocationRoot = $this->locationService->loadLocation($ezLocationRootId);
                $ezLocationRootPath = $ezLocationRoot->pathString;
                $subtreeCriterion = new Criterion\Subtree($ezLocationRootPath);
                $criteria[] = $subtreeCriterion;
            }
```

What is the purpose of the additional data stored in translationParameters?

This data can be used to define, for example if a breadcrumb of an eZNode should be clickable or hidden.

Example for not clickable breadcrumbs with bold text, if crumb.translationParameters.type == 'catalog':

**vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/Breadcrumbs/breadcrumb\_list.html.twig**

``` 
{% if not loop.last %}
  <li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
    {% if crumb.translationParameters.type == 'catalog' %}
      <b>
        <span itemprop="name">{{ crumb.text }}
        <meta itemprop="position" content="{{ loop.index }}"/>
      </b>
    {% else %}
      <a itemprop="item" href="{{ crumb.url }}">
        <span itemprop="name">{{ crumb.text }}
      </a>
      <meta itemprop="position" content="{{ loop.index }}" />
    {% endif %}
  </li>
{% else %}
  {# TODO make last element link configurable #}
  <li itemprop="itemListElement" itemscope itemtype="http://schema.org/ListItem">
    <span itemprop="name">{{ crumb.text }}
    <meta itemprop="position" content="{{ loop.index }}" />
  </li>
{% endif %}
```

![](attachments/23560760/23563701.png)  

## Templating

The Breadcrumbs is using a one template, which can be overridden if required. The template is loaded by a Controller which is called from the pagelayout. 

[Read more...](Breadcrumbs---Templates_23560532.html)

## Cookbook

In the cookbook you will find a tutorial how to extend own routes using the breadcrumb feature and how to extend the breadcrumb system using own generators. 

Find recipes in [cookbook...](Breadcrumbs---Cookbook_23560522.html)

## API

In this section you will find more information about the

  - general concept behind the breadcrumb system
  - a description of the breadcrumb generators used in the standard eZ Commerce (RoutesBreadcrumbsGenerator, EzContentBreadcrumbsGenerator, PostSilverModuleBreadcrumbsGenerator, RoutesBreadcrumbsGenerator)

[Read more](Breadcrumbs---API_23560525.html)

## Attachments:

![](images/icons/bullet_blue.gif) [FireShot Capture 168 - silver.e-shop - Next level B2B e-Commerc\_ - http\_\_\_harmony.local\_search\_query.png](attachments/23560964/23563570.png) (image/png)  
![](images/icons/bullet_blue.gif) [FireShot Capture 170 - silver.e-shop - Next level B2B e-Comm\_ - http\_\_\_harmony.local\_Produkt-Katal.png](attachments/23560964/23563569.png) (image/png)  
