# Autosuggest filters

In order to avoid displaying elements that may be hidden or restricted to certain users.

Autosuggest implements the following filters automatically. So if you find out that an element is not popping up, it may be because one of this filters:

## Ez Filters

- Document Type
- Permissions
- Language
- Visibility

``` php
$selectQuery
 ->createFilterQuery('document_type')
 ->setQuery('document_type_id:content');

 // eZ permission criterion filter
 $permissionsCriterion = $this->permissionsCriterionHandler->getPermissionsCriterion('content');
 $selectQuery
 ->createFilterQuery('ez_permissions_criterion')
 ->setQuery(
 $this->criterionVisitor->visit($permissionsCriterion)
 );

 // Language filter
 $selectQuery
 ->createFilterQuery('language')
 ->setQuery(
 'always_available_b:true OR meta_indexed_language_code_s:(' . implode(' ', $this->languages) . ')'
 );

 // Visibility filter
 $selectQuery
 ->createFilterQuery('visibility')
 ->setQuery('ses_invisible_b:false');
}
```

## Econtent Filters

- Document Type
- Catalog Segmentation
- Languages
- Visibility

``` php
$selectQuery
 ->createFilterQuery('document_type')
 ->setQuery('document_type_id:econtent');

if ($this->catalogSegmentationEnabled) {
 $catalogCodes = $this->catalogSegmentationService->getCatalogCodes();
 $solrValues = empty($catalogCodes) ? "''" : implode(' ', $catalogCodes);
 $selectQuery
 ->createFilterQuery('catalogCondition')
 ->setQuery('main_catalog_segments_ms:('.$solrValues.')');
}

$languages = $this->languages;
$language = (is_array($languages) && isset($languages[0])) ? $languages[0] : null;
if ($language) {
 $selectQuery
 ->createFilterQuery('language')
 ->setQuery('meta_indexed_language_code_s:'.$language);
}

// Visibility filter
$selectQuery
 ->createFilterQuery('visibility')
 ->setQuery('main_location_visible_b:true');
```
