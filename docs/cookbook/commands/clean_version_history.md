# Clean version history

## silversolutions:ez:clean-version-history

This is a command for the cleaning of eZ content object's version histories.

It deletes old versions of content objects. 

The objects to be cleaned can be specified with the filter options.

**Examples**

``` bash
# Remove single content objects
php bin/console silversolutions:ez:clean-version-history --siteaccess ezdemo_site_clean_admin --content-id 496 --content-id 497

# Remove content objects by filter
php bin/console silversolutions:ez:clean-version-history --siteaccess ezdemo_site_clean_admin --type-id 4 --subtree-path "/1/5/485/474/476/"
# Use the limit option to define how many versions should be removed 
# If no limit is provided all versions except for the 10 latest are removed
php bin/console silversolutions:ez:clean-version-history --siteaccess ezdemo_site_clean_admin --content-id 496 --content-id 497 --limit 10
```
