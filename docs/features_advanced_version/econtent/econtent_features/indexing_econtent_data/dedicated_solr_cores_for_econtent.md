# Dedicated solr cores for econtent

The main goal of dedicated solr cores for econtent data provider is to be able to import and re-index data (like products) using temporary solr core. In this case the application is not disturbed while the importer and indexer are working in 'tmp' cores.

After the processes are finished, the 'tmp' cores are swaped to the production cores and the application is using fresh data.

### Adapt configuration for 2 solr econtent cores

The standard configuration for eZ Commerce already is shipped using this configuration (see app/config/ezcommerce_advanced.yml). 

ezpublish/config/config.yml:

``` yaml
nelmio_solarium:
    endpoints:
        default:
            host: '%siso_search.solr.host%'
            port: '%siso_search.solr.port%'
            path: '%siso_search.solr.path%'
            core: '%siso_search.solr.core%'
            timeout: 30
        siso_core_admin:
            host: '%siso_search.solr.host%'
            port: '%siso_search.solr.port%'
            path: '%siso_search.solr.path%'
            core: admin
            timeout: 30
        siso_econtent:
            host: '%siso_search.solr.host%'
            port: '%siso_search.solr.port%'
            path: '%siso_search.solr.path%'
            core: econtent
            timeout: 30
        siso_econtent_back:
            host: '%siso_search.solr.host%'
            port: '%siso_search.solr.port%'
            path: '%siso_search.solr.path%'
            core: econtent_back
            timeout: 30
    clients:
        default:
            endpoints:
                - default
        econtent:
            endpoints:
                - siso_econtent
                - siso_econtent_back
                - siso_core_admin 
```

### Add parameters

app/config/parameters.yml:

``` yaml
parameters:
    siso_search.solr.host: 127.0.0.1
    siso_search.solr.port: 8983
    siso_search.solr.path: /solr
    siso_search.solr.core: collection1
    
```

### Manual changes in solr

eZ Commerce comes with an bash script which installs the solr engine and adapts the configuration for solr:

``` bash
bash ./install-solr.sh 8983
```
