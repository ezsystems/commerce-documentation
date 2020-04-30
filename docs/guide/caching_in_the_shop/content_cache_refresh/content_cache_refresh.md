# Content cache refresh

## Goal

The purpose of the content cache refreshing system is to be able to see actual information when objects in eZ Platform Backend are modified.

The content cache refresh service is used for these sitautions in the standard eZ Commerce:

- e.g. if one or many **navigation** nodes are changed, navigation cache will be refreshed.
- if a category is changed the cached names for categories used by the search will be updated (CategoryNameService)

!!! note

    In order to activate this feature a cronjob has to be set up.

## General

### Basic schema

!!! note

    Initially this mechanism was introduced to refresh different kinds of application caches (navigation service cache, translation cache). However, it can be used in other cases to handle content modifications

### Create system cronjob to run a console command: Set up cronjob to refresh caches

``` bash
php bin/console silversolutions:cache:refresh
```

## How to extend content cache refresh system

It is possible to extend cache refresh system with new content modification processors.

**Content modification processor** is a service which gets information about content modification and is able to do something using this data (e.g. refresh translation caches).

For example, if you have specific cache built using content objects, you can create new content modification processor service and implement a logic of cache refreshing there.

- [How to implement new content modification processor](content_cache_refresh_technical_implementation.md#how-to-implement-new-content-modification-processor) 
