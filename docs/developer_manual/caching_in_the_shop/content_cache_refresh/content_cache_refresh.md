# Content Cache Refresh

## Goal

The purpose of the content cache refreshing system is to be able to see actual information when objects in eZ Platform Backend are modified.

The content cache refresh service is used for these sitautions in the standard eZ Commerce:

  - e.g. if one or many **navigation** nodes are changed, navigation cache will be refreshed.
  - if a category is changed the cached names for categories used by the search will be updated (CategoryNameService)

In order to activate this feature a cronjob has to be setup\! See chapter 3 in this document

## General

### Basic schema

Initially this mechanism was introduced to refresh different kinds of application caches (navigation service cache, translation cache). However, it can be used in other cases to handle content modifications

### Create system cronjob to run a console command: Set up cronjob to refresh caches

``` 
php bin/console silversolutions:cache:refresh
```

## How to extend content cache refresh system

It is possible to extend cache refresh system with new content modification processors.

**Content modification processor** is a service which gets information about content modification and is able to do something using this data (e.g. refresh translation caches).

For example, if you have specific cache built using content objects, you can create new content modification processor service and implement a logic of cache refreshing there.

  - [How to implement new content modification processor](Content-cache-refresh.-Technical-implementation_23560554.html#Contentcacherefresh.Technicalimplementation-Contentcacherefresh.Technicalimlementation-Howtoimplementnewcontentmodificationprocessor)

## Attachments:

![](images/icons/bullet_blue.gif) [Screenshot from 2015-03-11 14:54:41.png](attachments/23560351/23563379.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-03-11 15:00:52.png](attachments/23560351/23563340.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-03-11 15:00:19.png](attachments/23560351/23563380.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2015-03-11 15:00:52.png](attachments/23560351/23563381.png) (image/png)  
![](images/icons/bullet_blue.gif) [Set up triggers.png](attachments/23560351/23563361.png) (image/png)  
![](images/icons/bullet_blue.gif) [Create workflow.png](attachments/23560351/23563363.png) (image/png)  
![](images/icons/bullet_blue.gif) [Content refresh system basich diagramm.xml](attachments/23560351/23563499.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Content refresh system basich diagramm.xml.png](attachments/23560351/23563502.png) (image/png)  
![](images/icons/bullet_blue.gif) [Content refresh system basich diagramm.xml.png](attachments/23560351/23563353.png) (image/png)  
![](images/icons/bullet_blue.gif) [Content refresh system basich diagramm.xml](attachments/23560351/23563501.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Content refresh system basich diagramm.xml](attachments/23560351/23563362.xml) (application/drawio)  
![](images/icons/bullet_blue.gif) [Content refresh system basich diagramm.xml.png](attachments/23560351/23563500.png) (image/png)  
