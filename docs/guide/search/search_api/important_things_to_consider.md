# Important things to consider

## Limitations when executing a sub search inside a regular search

When a search is queried for Econtent (for Example via `\Siso\Bundle\SearchBundle\Service\EcontentSolrEshopSearch::searchProducts()`) the `EcontentSearchTermConditionHandler` memorizes the query fields (list of field to consider in search). This is intentional since for the search, the boosting fields and the field restrictions must be merged.

Unfortunately, this influences additional search queries within the same PHP process (HTTP request or CLI call). The 2nd query "derives" the query fields from the 1st, the 3rd from 2nd and 1st, etc.

### Example

Perform a sub search while in the middle of a bigger search: I.E.: Searching for individual elements content while searching:

``` 
Example:
Search for all products with boosting fields from configuration 
    With each search result search for something else, like related products or category images. In this search you should know that all search boosting fields from the main search will be taken into consideration if you use the search term condition handler.
```

This behavior is connected to <http://rm.extranet.silversolutions.de/issues/11714>
