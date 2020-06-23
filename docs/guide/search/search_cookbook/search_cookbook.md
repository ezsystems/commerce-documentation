# Search cookbook

## Configuring a new tab in search results

To show a new tab in the search result page, add configuration for a new group.

For example, you can add a new `download` tab with content information:

``` yaml
siso_search.default.groups.search:
    ...
    download:
        types:
            - download
        path: '/1/43/'
        section: 3
        visibility: true 
 
siso_search.default.sort:
    ...
    download:
        - score
        - name|asc
        - name|desc
 
siso_search.default.sort.preferred:
    ...
    download: score
```
