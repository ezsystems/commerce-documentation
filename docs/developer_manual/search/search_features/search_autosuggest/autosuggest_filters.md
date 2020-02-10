#  Autosuggest Filters 

In order to avoid displaying elements that may be hidden or restricted to certain users.

Autosuggest implements the following filters automatically. So if you find out that an element is not popping up, it may be because one of this filters:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th> </th>
</tr>
</thead>
<tbody>
<tr>
<td><h3 id="AutosuggestFilters-EzFilters">Ez Filters</h3>
<p>Document Type</p>
<p>Permissions</p>
<p>Language</p>
<p>Visibility</p></td>
<td><pre><code> $selectQuery
 -&gt;createFilterQuery(&#39;document_type&#39;)
 -&gt;setQuery(&#39;document_type_id:content&#39;);

 // eZ permission criterion filter
 $permissionsCriterion = $this-&gt;permissionsCriterionHandler-&gt;getPermissionsCriterion(&#39;content&#39;);
 $selectQuery
 -&gt;createFilterQuery(&#39;ez_permissions_criterion&#39;)
 -&gt;setQuery(
 $this-&gt;criterionVisitor-&gt;visit($permissionsCriterion)
 );

 // Language filter
 $selectQuery
 -&gt;createFilterQuery(&#39;language&#39;)
 -&gt;setQuery(
 &#39;always_available_b:true OR meta_indexed_language_code_s:(&#39; . implode(&#39; &#39;, $this-&gt;languages) . &#39;)&#39;
 );

 // Visibility filter
 $selectQuery
 -&gt;createFilterQuery(&#39;visibility&#39;)
 -&gt;setQuery(&#39;ses_invisible_b:false&#39;);
}</code></pre></td>
</tr>
<tr>
<td><h3 id="AutosuggestFilters-EcontentFilters">Econtent Filters</h3>
<p>Document Type</p>
<p>Catalog Segmentation</p>
<p>Languages</p>
<p>Visbility</p>
<p> </p></td>
<td><pre><code>$selectQuery
 -&gt;createFilterQuery(&#39;document_type&#39;)
 -&gt;setQuery(&#39;document_type_id:econtent&#39;);

if ($this-&gt;catalogSegmentationEnabled) {
 $catalogCodes = $this-&gt;catalogSegmentationService-&gt;getCatalogCodes();
 $solrValues = empty($catalogCodes) ? &quot;&#39;&#39;&quot; : implode(&#39; &#39;, $catalogCodes);
 $selectQuery
 -&gt;createFilterQuery(&#39;catalogCondition&#39;)
 -&gt;setQuery(&#39;main_catalog_segments_ms:(&#39;.$solrValues.&#39;)&#39;);
}

$languages = $this-&gt;languages;
$language = (is_array($languages) &amp;&amp; isset($languages[0])) ? $languages[0] : null;
if ($language) {
 $selectQuery
 -&gt;createFilterQuery(&#39;language&#39;)
 -&gt;setQuery(&#39;meta_indexed_language_code_s:&#39;.$language);
}

// Visibility filter
$selectQuery
 -&gt;createFilterQuery(&#39;visibility&#39;)
 -&gt;setQuery(&#39;main_location_visible_b:true&#39;);</code></pre></td>
</tr>
</tbody>
</table>
