# Update notes silver.eShop 

Please check <https://doc.ezplatform.com/en/latest/releases/updating_ez_platform/> for details about how to update silver.eShop.

**Important**:  **From your new update branch add meta repo of silver.eshop**
``` 
git remote add upstream http://gitlab.silversolutions.de:8081/ssl/silver-eshop-meta.git
```
## Updating from silver.eShop Version 3 to 4.1

### Check API Changes 3.7 → 4.1.1

  - Silversolutions\\Bundle\\EshopBundle\\Controller\\SearchController wurde verschoben nach Siso\\Bundle\\SearchBundle\\Controller\\SearchController

  - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Services/DataProvider/EcontentCatalogDataProvider.php constructor enthält zusätzliche Parameter

  - vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Services/WebConnectorErpService.php constructor enthält zusätzliche Parameter

### Required bundles for migration

  - add Xmltext Fieldtype: `composer ``require` `--no-update ``"ezsystems/ezplatform-xmltext-fieldtype:^1.3.0"`

Migrate XML text to Richtext

siehe <https://doc.ezplatform.com/en/latest/migrating/migrating_from_ez_publish_platform/#321-migrate-xmltext-content-to-richtext>

<table>
<tbody>
<tr class="odd">
<td><p><code class="sourceCode php">php -d memory_limit=1536M bin/console ezxmltext:convert-to-richtext --dry-run --export-<span class="fu">dir=ezxmltext-export --export-<span class="fu">dir-filter=notice<span class="ot">,warning<span class="ot">,<span class="kw">error --concurrency <span class="dv">4 -v</code></p>
<p><br />
</p></td>
</tr>
</tbody>
</table>

Migrate matrix fields

bin/console ezplatform:migrate:legacy\_matrix

### Update database

``` 
mysql -h <host> -u <user> -p <database> < vendor/ezsystems/ezpublish-kernel/data/update/mysql/dbupdate-5.4.0-to-6.13.0.sql
mysql -h <host> -u <user> -p <database> <  vendor/ezsystems/date-based-publisher/bundle/Resources/install/datebasedpublisher_scheduled_version.sql
mysql -h <host> -u <user> -p <database> < vendor/ezsystems/flex-workflow/bundle/Resources/install/flex_workflow.sql
mysql -h <host> -u <user> -p <database> < vendor/ezsystems/ezplatform-workflow/src/bundle/Resources/install/schema.sql
```

### ContentTypes changes

<table>
<thead>
<tr class="header">
<th>ContentType</th>
<th>Changes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>User</td>
<td><p>Add fields first_name, last_name und ses_customer_group (for definition check current silver.eshop installation!)</p></td>
</tr>
<tr class="even">
<td>Feature link (former st module)</td>
<td><div class="content-wrapper">
<p>Change Attribute "parameters" from ezmatrix to textline</p>
</td>
</tr>
</tbody>
</table>

### Adapt config

Adapt location ids according to your DB

``` 
silver_tools.default.translationFolderId: 93
siso_core.default.user_group_location: 5
siso_core.default.user_group_location.business: 12
siso_core.default.user_group_location.private: 12
```

### ERP settings

Starting from silver.eShop v4.1.1 the TSOWebConnector is configured as default class. In order to use the classic webconnector you need to configure:

``` 
silver_erp.message_transport.class: Silversolutions\Bundle\EshopBundle\Services\Transport\WebConnectorMessageTransport
```

### Roles and policy changes

Check roles for Anonymus and customers and compar to standard. Replace old legacy roles by new ones.

Note: there is a new role for quickorder\!

![image2019-5-7\_9-29-22.png](http://confluence.extranet.silversolutions.de:8090/download/attachments/45023370/image2019-5-7_9-29-22.png?version=1&modificationDate=1557214160000&api=v2)

Check user content read self rights as well\!

![](http://confluence.extranet.silversolutions.de:8090/download/attachments/45023370/image2019-5-7_9-30-11.png?version=1&modificationDate=1557214209000&api=v2)

### Replace old codes in twig if used:

{% set user\_has\_role = has\_user\_role('ses\_legacy', 'write\_basket') %} → 

    {% set user_has_role = has_user_role('siso_policy', 'write_basket') %}
