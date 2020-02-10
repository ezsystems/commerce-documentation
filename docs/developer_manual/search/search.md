#  Search 

eZ Commerce comes with a very powerful search engine. The search engine is capable to provide one common search using product data and content. 

Product data and content is indexed automatically using the build in search engine based on Solr. 

The search displays the results in different groups. A group can consist products, videos, downloads, etc.

![](attachments/23560778/23563925.png)

products in the search result

![](attachments/23560778/23563927.png)

content in the search result

Facets can be grouped and the user can remove all facets of a group with one click: 

![](attachments/23560778/23563926.png)

Features of the search:

  - Groups displayed in tabs such as products and content can be configured.
  - Facets are build dynamically as defined in configuration.
  - User can use the back button to go back as URL's are unique.
  - The search is using ajax to refresh the list.
  - The data which is used for facets can be indexed in a project specific indexer plugin.
  - You can use your back button of the browser without leaving the search
  - a url to a search result can be send via email to a friend
  - Boosting can be defined in the configuration
  - On the search result page it can be configured wether the facets shall be shown on top of the list or in the left column
  - The products can be shown in a list or gallery view

The search bases on the new Solr implementation of the CMS eZ Platform (introduced in version 5.4.5). eZ Commerce extends the features of the search implementation with special features required for product search.

It also comes with an extension plugin system which allows to index custom fields by Contenttype. 

### Autosuggest

eZ Commerce provides a user friendly [autosuggest](Search-Autosuggest_23560726.html) feature:

![](attachments/23560778/23563535.png)

### Important commands

**Reindex eZ Content**

``` 
php -d memory_limit=-1 bin/console ezplatform:solr_create_index
```

## Important terms used in this document

Solr

Solr is highly reliable, scalable and fault tolerant, providing distributed indexing, replication and load-balanced querying, automated failover and recovery, centralized configuration and more. Solr powers the search and navigation features of many of the world's largest internet sites. 

<http://lucene.apache.org/solr/>

Facet

A facet is a collection of attributes provided by a product or content. 

A facet could be e.g. the color or a price range. Example:

![](attachments/23560675/23563321.png)

Facet showing the manufactures

A facet is build dynamically based on the current search result. 

## Before you start 

Please keep in mind that search is really connected with a lot of different modules in our shop. Be sure to check these out:

  - [Catalog Element](http://confluence.ng.silverproducts.de/)

The installation of the search engine is described in the chapter [Installation](Installation_23561045.html)

## Definitions

Search phrase: it is a text input provided by the user to perform the search query.

Facet: A defined filter to narrow the search. Every facet group acts like an AND and every individual facet can be configured as Single, Multi Or or Multi And.

### Example

Given this scenario: 

Facet Category is a Multi\_Or and has this options **Category A**, **Category B**.

Facet Colors is a Multi\_AND and has this options **Color Red** and **Color Silver.** 

Facet Watt is defined Single and has this option: **1000**

Results elements = (Element has Category A **OR** Category B) **AND** (Element is Red **AND** Element is Silver) **AND** (Element has 1000 Watt)

Multi\_And facets are good to handle elements with multiple features at the same time. Like an element that has 2 or more colors.

**Table of contents:**

  - [Autosuggest](#Search-Autosuggest)
  - [Important commands](#Search-Importantcommands)

[Important terms used in this document](#Search-Importanttermsusedinthisdocument)

[Before you start ](#Search-Beforeyoustart)

[Definitions](#Search-Definitions)

  - [Example](#Search-Example)

[FAQ](#Search-FAQ)

[Templating](#Search-Templating)

[Cookbook](#Search-Cookbook)

[API](#Search-API)

-----

**Child Pages:**

  - [Search - Features](Search---Features_23560623.html)
  - [Search - API](Search---API_23560412.html)
  - [Search - Templates](Search---Templates_23560663.html)
  - [Search - Cookbook](Search---Cookbook_23560665.html)
  - [Search - FAQ](Search---FAQ_23560674.html)
  - [Search - Configuration](Search---Configuration_23560662.html)

## FAQ

Which Solr version is used?

eZ Commerce is using the Solr recommended by eZ systems. You can follow the instruction from eZ or check chapter [Installation](https://doc.silver-eshop.de/display/EZC14/Installation)

If you are using econtent as a storage engine 2 additional cores in Solr are required.

The shop is using the new searchengine ezplatform search.

What are the search limitations?

See [Important Things to Consider](https://doc.silver-eshop.de/display/EZC14/Important+Things+to+Consider)

How to change facet position on the page?

There are 2 possibilities for facet design on search result page:

  - center - search result page has full width and facets are centered just above search results
  - left - facets appear on the search result page in the left column as a block.

Please change configuration to your choice

``` 
# position of facet block in search. Possible values: left, center
siso_search.default.facet_position: center
```

How can I configure sorting for search?

 There are 2 configurations that handle sorting on search pages:

  - **siso\_search.default.sort** - define the list of all available sorting options per group

  - **siso\_search.default.sort.preferred** - define default sorting option per group

``` 
siso_search.default.sort:
    product_list:
        - score
        - name|asc
        - name|desc
        - sku|asc
        - sku|desc
        - price|asc
        - price|desc
    product:
        - score
        - name|asc
        - name|desc
        - sku|asc
        - sku|desc
        - price|asc
        - price|desc
    content:
        - score
        - name|asc
        - name|desc
    files:
        - score
        - name|asc
        - name|desc

siso_search.default.sort.preferred:
    product_list: score
    product: score
    content: score
    files: score
```

If there is a need for new sorting option then go to : [Search - Cookbook\#Cookbook-Howtoimplementnewsortingoptionforsearch](https://doc.silver-eshop.de/display/EZC14/Search+-+Cookbook#Search-Cookbook-Cookbook-Howtoimplementnewsortingoptionforsearch)

How can I change facets display?

There are 3 parameters that can tweak the facet display

See : [Search - Templates](https://doc.silver-eshop.de/display/EZC14/Search+-+Templates)

How can I change pagination limits for search result?

 Depending on chosen design layout we can configure pagination limit and default limit:

  - **siso\_search.default.limits** - define the list of all available limit options per design
  - **siso\_search.default.limits.preferred** - define default limit option per design

``` 
siso_search.default.limits:
    left:
        3: 3
        6: 6
        12: 12
        24: 24
        48: 48
    center:
        4: 4
        8: 8
        16: 16
        32: 32
        64: 64

siso_search.default.limits.preferred:
    left: 6
    center: 8
```

How can i add synonyms for search?

Synonyms is a very handy feature that allows different phrase input that have the same meaning.

Example:

You have products with gigabyte specification like a 250 gigabyte hard drive. But we cannot expect users to write gigabyte the same way we do, or even the correct way. So we can add synonyms for gigabyte:

gigabyte, giga byte, gb, Gb, gigab, gbyte and so on.

Now every time the user search for any of those gigabyte synonyms, the search engine will find also all the synonyms.

User search for: 1 gb

Search engine will return 1 gigabyte.

The synonyms are defined in file synonyms.txt which can be found inside Solr collection directory/conf

**Please note that after changing synonyms.txt file you have to restart solar to make the changes take effect.**

Do i need different cores for different languages

 We recommend to use different cores for languages since some features such as stemming or synonyms might need a special configuration.

Where do i find a log which queries were done by users?

 Currently the search querys are logged in a database table "ses\_log\_search" using the logger concept (see [Logging Cookbook For Database Logging](#)):

Example data from the table:

``` 
+----+---------------------+---------------------+-----------+-------------+-------------------+----------------------------+---------+---------+----------+
| id | log_timestamp       | log_channel         | log_level | log_message | request_id        | session_id                 | user_id | results | language |
+----+---------------------+---------------------+-----------+-------------+-------------------+----------------------------+---------+---------+----------+
|  1 | 2016-01-27 19:03:19 | silver_eshop_search |       200 | mc          | smj2kgl1-667e75a0 | smj2kgl1pcbeb2rgroatnu7du6 | 10      |       3 | ger-DE   |
|  2 | 2016-01-27 19:03:41 | silver_eshop_search |       200 | speaker     | smj2kgl1-67dcb267 | smj2kgl1pcbeb2rgroatnu7du6 | 10      |       1 | ger-DE   |
|  3 | 2016-01-27 19:04:03 | silver_eshop_search |       200 | audio       | smj2kgl1-6933d4b4 | smj2kgl1pcbeb2rgroatnu7du6 | 10      |      17 | ger-DE   |
|  4 | 2016-01-27 19:04:18 | silver_eshop_search |       200 | audio       | smj2kgl1-6a2e3dcf | smj2kgl1pcbeb2rgroatnu7du6 | 10      |      17 | ger-DE   |
|  5 | 2016-01-27 19:04:28 | silver_eshop_search |       200 | audio       | smj2kgl1-6ac91c2d | smj2kgl1pcbeb2rgroatnu7du6 | 10      |      17 | ger-DE   |
|  6 | 2016-01-27 19:04:59 | silver_eshop_search |       200 | audio       | smj2kgl1-6cb28820 | smj2kgl1pcbeb2rgroatnu7du6 | 10      |      17 | ger-DE   |
|  7 | 2016-01-27 19:05:05 | silver_eshop_search |       200 | audio       | smj2kgl1-6d1be111 | smj2kgl1pcbeb2rgroatnu7du6 | 10      |      17 | ger-DE   |
|  8 | 2016-01-27 19:07:38 | silver_eshop_search |       200 | mc-8        | smj2kgl1-76a2110b | smj2kgl1pcbeb2rgroatnu7du6 | 10      |      12 | ger-DE   |
|  9 | 2016-01-27 19:07:48 | silver_eshop_search |       200 | mc-8        | smj2kgl1-774af307 | smj2kgl1pcbeb2rgroatnu7du6 | 10      |      12 | ger-DE   |
| 10 | 2016-01-27 19:08:05 | silver_eshop_search |       200 | mc-8        | smj2kgl1-785b5597 | smj2kgl1pcbeb2rgroatnu7du6 | 10      |      12 | ger-DE   |
+----+---------------------+---------------------+-----------+-------------+-------------------+----------------------------+---------+---------+----------+
```

I am getting the error: The service "siso\_search.econtentsolr\_search\_service" has a dependency on a non-existent service "solarium.client.econtent".

 Please check the Solarium settings and add a setting for the econtent based search:

``` 
nelmio_solarium:
    endpoints:
        default:
            host: '%siso_search.solr.host%'
            port: '%siso_search.solr.port%'
            path: '%siso_search.solr.path%'
            core: '%siso_search.solr.core%'
            timeout: 30
    clients:
        default:
            endpoints:
                - default
        econtent:
            endpoints:
                - default
```

How do I enable character escaping for user's search terms input?

If you are using econtent as catalog data provider you can configure how Solr special characters are evaluated.

``` 
# Configuration to escape or remove Solr special characters from search query.
# Possible values: escape, remove or empty.
siso_search.default.solr_special_characters: escape
```

  - **escape**: will escape every Solr special character from the user search query.
  - **remove**: will remove every Solr special character from the user search query.
  - **\~**: will keep all Solr special characters.

These are the current Solr special characters:

``` 
+ - && || ! ( ) { } [ ] ^ " ~ * ? : \
```

Every character listed above has a special function and required syntax in Solr if they are not escaped.

Here are some examples:

'-' not operator, when preceding a word token.   
  
'+' operator, when preceding a word token implies an AND operator. The term after the + symbol definitely exists somewhere in the documents searched.   
  
'\*' wildcard operator when followed by or preceded by a word token. Use wildcards to look for spelling variations and alternate word endings.   
  
() characters serve to group tokens with AND/OR operator. Search terms within parentheses are read first, then terms outside parentheses is read next.   
  
" operator surrounding word tokens will cause the word tokens to be treated as is and as a phrase.

How do I enable catalog segmentation filters for Econtent search?

 There is no special setting for the search regarding catalog segmentation. As soon as it is enabled for Econtent, it is also added to the search filters.

``` 
parameters:
    silver_econtent.default.section_filter: enabled
```

How to avoid that ezplatform:solr\_create\_index removes the index in production?

When eZ is used as dataprovider how can be avoided that during index time the shop has not a complete solr index?

It depends to the configuration. If Solr is configured to auto-commit, the index will be removed. If no auto-commit is configured, the index will be removed as well, **but** it will not be effective and everything will be searchable until the commit at the end. No auto-commit, though, means also that changes in the administration will not be visible, since the process does not include a commit.

A simple solution would be to increase the auto-commit to a value, close to the needed time of the indexing process. But this is only applicable to set-ups with data, which do not take hours to index. Changes in the administration would, because of that, also be only visible after that amount of time.

Please check your solrconfig.xml of the respective Solr core and adjust the settings:

``` 
   <maxTime>${solr.autoCommit.maxTime:160000}</maxTime>
       <openSearcher>true</openSearcher>
     </autoCommit>
```

When i update prios in eZ backend the changes are not applied to the search index

Currently when editor changes the Location Priority in the backend, this one is not automatically indexed in solr.  Please check [Recommended patches](/pages/createpage.action?spaceKey=EZC14&title=Recommended+patches&linkCreation=true&fromPageId=23560674) and the description of the issue (<https://github.com/netgen/ezplatformsearch/issues/11>)

## Templating

The search is using a list of templates which can be overridden if required. 

[Read more...](Search---Templates_23560663.html)

## Cookbook

Find recipes in [cookbook...](Search---Cookbook_23560665.html)

## API

Unable to render {include} The included page could not be found.

## Attachments:

![](images/icons/bullet_blue.gif) [image2016-2-4 11:20:13.png](attachments/23560778/23563323.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-2-18 16:39:27.png](attachments/23560778/23563925.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-2-18 16:39:58.png](attachments/23560778/23563927.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-2-18 16:47:31.png](attachments/23560778/23563926.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-9-28 21:10:24.png](attachments/23560778/23563535.png) (image/png)  
