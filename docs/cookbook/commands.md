# Commands

## Refreshing the navigation cache

eZ Commerce creates a navigation cache which contains all categories and menu items in a cache.
This cache is updated if a menu item is updated. The following command updates the cache per SiteAccess:

``` bash
php bin/console silversolutions:navigation_cache:refresh --siteaccess=de
```

## Removing Content item versions

`silversolutions:ez:clean-version-history` cleans Content item version history.
It deletes old versions of Content items. 

The content to be cleaned can be specified with the filter options.

``` bash
php bin/console silversolutions:ez:clean-version-history --type-id=4
```

For example:

``` bash
# Remove single Content items
php bin/console silversolutions:ez:clean-version-history --siteaccess ezdemo_site_clean_admin --content-id 496 --content-id 497

# Remove Content items by filter
php bin/console silversolutions:ez:clean-version-history --siteaccess ezdemo_site_clean_admin --type-id 4 --subtree-path "/1/5/485/474/476/"
# Use the limit option to define how many versions should be removed 
# If no limit is provided, all versions except the last 10 are removed
php bin/console silversolutions:ez:clean-version-history --siteaccess ezdemo_site_clean_admin --content-id 496 --content-id 497 --limit 10
```

## Generating assets

`silversolutions:cache-asset:generate` creates cache for assets using the current data provider.


``` bash
php bin/console silversolutions:cache-asset:generate
```

The command collects all products, loops through them and recreates the asset cache.
