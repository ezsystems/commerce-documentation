# Search FAQ

## Which Solr version is used?

eZ Commerce uses the Solr version [recommended by Ibexa](https://doc.ezplatform.com/en/latest/getting_started/requirements/).

If you are using eContent as storage engine, two additional cores in Solr are required.

## How can I add synonyms for search?

Synonyms enable different phrase inputs that have the same meaning.

For example, you have products with gigabyte specification, like a 250 gigabyte hard drive.
You cannot expect users to spell out "gigabyte", so you can add synonyms for the word:
"gigabyte", "giga byte", "gb", "Gb", "gigab", "gbyte", and so on.

Every time the user search for any of those synonyms, the search engine also finds all the synonyms.

User search for: "1 gb" returns "1 gigabyte".

The synonyms are defined in `synonyms.txt` file which can be found inside Solr collection `directory/conf`.

After changing `synonyms.txt` you have to restart Solr.

## Do I need different cores for different languages?

We recommend using different cores for languages because some features such as stemming or synonyms can need special configuration.

## Where can I find a log of search queries?

Search queries are logged in a database table `ses_log_search` using a logger.

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

## Non-existent service "solarium.client.econtent"

If you see an error `The service "siso_search.econtentsolr_search_service" has a dependency on a non-existent service "solarium.client.econtent".`,
check the Solarium settings and add a setting for eContent-based search:

``` yaml
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

## Avoid removing index in production

When the content model is used as data provider, `ezplatform:solr_create_index` removes the index in production.

It depends ono the configuration. If Solr is configured to auto-commit, the index is removed.
If no auto-commit is configured, the index is removed as well, but the removal only takes effect after the commit at the end. Until that time everything is still searchable.
No auto-commit also means that changes in the administration are be visible, since the process does not include a commit.

To resolve this, you can increase the auto-commit time to a value close to the needed time of the indexing process.
This is only applicable for setups with data that does not take very long to index.

Check `solrconfig.xml` of the respective Solr core and adjust the settings:

``` xml
<maxTime>${solr.autoCommit.maxTime:160000}</maxTime>
   <openSearcher>true</openSearcher>
 </autoCommit>
```

## Updating priority in th Back Office does not apply in search box

When a user changes the Location priority in the Back Office, the new priority is not automatically indexed in Solr.
