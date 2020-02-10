#  Cluster setup 

![](http://ezcommerce-demo.ezplatform.com/bundles/silversolutionseshop/img/logo.png)

The cluster is based on Docker, all images are available on our private docker registry, but in order to create the containers locally is sufficient to use Docker Compose.

Add the following content to a new file in **app/config/cluster.yml** directory:

``` 
# set the handlers
ezpublish:
    system:
        default:
            io:
                metadata_handler: dfs
                binarydata_handler: nfs
 
# declare the handlers
ez_io:
    binarydata_handlers:
        nfs:
            flysystem:
                adapter: nfs_adapter
    metadata_handlers:
        dfs:
            legacy_dfs_cluster:
                connection: doctrine.dbal.dfs_connection
 
# new doctrine connection for the dfs legacy_dfs_cluster metadata handler.
doctrine:
    dbal:
        connections:
            dfs:
                driver: pdo_mysql
                host: 192.168.2.243
                port: 3306
                dbname: harmony_dev_cluster
                user: remote
                password: <password for remote (see Wiki)>
                charset: UTF8
 
# new flysystem adapter for the nfs metadata handler
oneup_flysystem:
    adapters:
        nfs_adapter:
            local:
                # The last part, $var_dir$/$storage_dir$, is required for legacy compatibility
                directory: "/mnt/data/$var_dir$/$storage_dir$"
```

include it in a copy of **config\_prod.yml** saved to **app/config/config\_cluster.yml** and create a new **ezpublish\_cluster.yml** file with the following content:

``` 
imports:
    -
        resource: ezpublish_prod.yml

ezpublish:
    http_cache:
        # As of 5.4 only use "http"
        # "single_http" and "multiple_http" are deprecated but will still work.
        purge_type: http

    system:
        # Assuming that my_siteaccess_group your frontend AND backend siteaccesses
        ezdemo_site_access_group:
            http_cache:
                purge_servers:
                    - 'http://127.0.0.1:8080'
        engl:
            languages:
                - eng-GB
                - eng-US
            http_cache:
                purge_servers:
                    - 'http://127.0.0.1:8080'
        ger:
            languages:
                - ger-DE
            http_cache:
                purge_servers:
                    - 'http://127.0.0.1:8080'
```

configure path to the central image storage in your parameters.yml:

``` 
ses_eshop.cluster_path: /mnt/data
```

Inside this folder the "var" folder has to be placed\!

A symlink insede of one of the two web container has to be created in order to deliver contents from /mnt/data directly:

**Example**

``` 
ln -s /mnt/data /var/www/projects/harmony_web/cluster_share
```
