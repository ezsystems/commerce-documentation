#  econtent - Features 

The econtent dataprovider offers a variety of features. Some of the features will be described in detail on separate pages.

## Overview

Feature

eZ Commerce (Advanced version only) can use either products stored in eZ or in econtent

The dataprovider which shall be used can be switched by configuration. A command automises the switch: [Switching the data provider](Switching-the-data-provider_23560870.html)

Build a tree containing different objects such as e.g. categories and products

The tree can have a unlimited depth (max int)

url alias system

Each object in the tree has a unique and readable URL. Currently the URL cannot be translated. This feature is planned for 2019

a product can be placed in more than one location

An object can be hidden

This feature will hide one object only. If products are assigned to a category which is hidden they will be still found using search.

An object can have a section

The section can be used to separate one product catalog for different shops

An object belongs to a class. A class can define one ore more attributes

See [econtent - backend features](econtent---backend-features_23560203.html)

Attributes can be translated to different languages

Segmentation of catalogs

A catalog can be segmented in order to filter a catalog by a user group or by a user directly.

See [Econtent dataprovider - catalog segmentation](Econtent-dataprovider---catalog-segmentation_23560824.html)

Easy and fast imports

econtent is build using a simple database structure: [Econtent dataprovider - database model](Econtent-dataprovider---database-model_23560823.html) . The objects and attributes are stored in 2 DB tables.

Imports can be done by directly using DB import features (e.g. direct conversion using XSLT or other fast processors) or using an API (based on doctrine or direct DB access).

Staging feature

econtent offers a staging feature which allows to import a catalogue without affecting the production system. Details see [econtent - Staging system](econtent---Staging-system_23561079.html)

Search

The catalog stored in econtent is indexed in dedicated Solr cores: [Indexing econtent data](Indexing-econtent-data_23560588.html)

The integrated search implementation will use the live core defined for econtent.

Extending search

The indexer for econtent offers a plugin feature. More complex search requirements can be implemented using a indexerplugin: [How to implement a custom indexer plugin for econtent](How-to-implement-a-custom-indexer-plugin-for-econtent_23560600.html)
