# Commands

These are the most important commands for eZ Commerce. Some of the command are provided by Symfony or eZ Platform directly. 

## Generate assetics or create symlinks for new bundles (Symfony)

In case Javascript or CSS has been change the following command has to be executed:

``` bash
php bin/console assetic:dump --env=prod
```

If a new bundle has been introudced you have to create the smylinks to deploy assets, js and css to the public web folder

``` bash
php bin/console assets:install --symlink --relative --env=prod
```

## Clear cache (Symfony)

If you are not running in dev mode and change made in twig templates or settings have to be activated please run this command

``` bash
php bin/console cache:clear --env=prod
```

## Index Solr (eZplatform)

Usually the Solr index should be in sync with your data stored in the CMS. In case of changed connected to IndexPlugins a reindexing might be required:

``` bash
php bin/console ezplatform:solr_create_index --env=prod
```

## Remove anonymous baskets

If a user creates a basket but does not have a login the basket will be assigned using the session id. If a session expires the basket will not automatically be removed. The following command will remove anonymous baskets older than 30 days (=720 hours): 

``` bash
php bin/console silversolutions:baskets:clear 720
```

## Refresh the navigation cache

eZ Commerce creates a navigation cache which contains all categories and menu items (CMS) in a cache. This cache will be updated in case a menu item has been updated. The command remove update the cache per siteaccess. 

``` bash
 php bin/console silversolutions:navigation_cache:refresh --siteaccess=de
```

## Remove content objects versions

In  former versions of eZ Platform the core does not restrict the number of versions the CMS creates after an objects has been publish. Since eZ Commerce updates the user record after each login the object versions for users could be very high. This command will remove the not used versions (the number of remaining versions can be configured). The id=4 in the following example will remove user versions. 

``` bash
silversolutions:ez:clean-version-history --type-id=4
```
