# Update notes silver.eShop

Please check [Updating eZ Platform](https://doc.ezplatform.com/en/latest/releases/updating_ez_platform/) for details about how to update silver.eShop.

**Important**: From your new update branch add meta repo of silver.eshop

``` 
git remote add upstream http://gitlab.silversolutions.de:8081/ssl/silver-eshop-meta.git
```

## Updating from silver.eShop Version 3 to 4.1

### Check API Changes 3.7 -> 4.1.1

- `Silversolutions\Bundle\EshopBundle\Controller\SearchController` was changed to `Siso\Bundle\SearchBundle\Controller\SearchController`
- `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Services/DataProvider/EcontentCatalogDataProvider.php` constructor contains additional parameters
- `vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Services/WebConnectorErpService.php` constructor contains additional parameters

### Required bundles for migration

- add Xmltext Fieldtype: `composer require --no-update "ezsystems/ezplatform-xmltext-fieldtype:^1.3.0"`

Migrate XML text to Richtext

See [Migrating from eZ Publish Platform](https://doc.ezplatform.com/en/latest/migrating/migrating_from_ez_publish_platform/#321-migrate-xmltext-content-to-richtext).

```
php -d memory_limit=1536M bin/console ezxmltext:convert-to-richtext --dry-run --export-dir=ezxmltext-export --export-dir-filter=notice,warning,error --concurrency 4 -v
```

Migrate matrix fields

```
bin/console ezplatform:migrate:legacy_matrix
```

### Update database

``` 
mysql -h <host> -u <user> -p <database> < vendor/ezsystems/ezpublish-kernel/data/update/mysql/dbupdate-5.4.0-to-6.13.0.sql
mysql -h <host> -u <user> -p <database> <  vendor/ezsystems/date-based-publisher/bundle/Resources/install/datebasedpublisher_scheduled_version.sql
mysql -h <host> -u <user> -p <database> < vendor/ezsystems/flex-workflow/bundle/Resources/install/flex_workflow.sql
mysql -h <host> -u <user> -p <database> < vendor/ezsystems/ezplatform-workflow/src/bundle/Resources/install/schema.sql
```

### ContentTypes changes

|ContentType|Changes|
|--- |--- |
|User|Add fields first_name, last_name und ses_customer_group (for definition check current silver.eshop installation!)|
|Feature link (former st module)|Change Attribute "parameters" from ezmatrix to textline|

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

Check roles for Anonymous and customers and compare to standard. Replace old legacy roles by new ones.

Note: there is a new role for Quickorder.

Check user content read self rights as well.

### Replace old codes in twig if used:

`{% set user_has_role = has_user_role('ses_legacy', 'write_basket') %}`

-> 

`{% set user_has_role = has_user_role('siso_policy', 'write_basket') %}`
