#  Search - Cookbook 

# How to implement own custom search 

The is an example of how to implement custom search for our current shop API.

We are going to create a new service which will search for products that :

  - are discontinued,
  - are visible,
  - set the sorting option to "relevance"
  - add facet about manufacturers for filtering
  - boost some of the fields (which means that they will be more important than others)

## Steps

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><p>We will use our current implementation of search interface, which has all necessary methods for searching using eZ solr gateway.</p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>class EzSolrEshopSearch implements EshopSearchInterface</code></pre>
</td>
</tr>
<tr>
<td><p>We will create new method in the controller called "searchDiscontinuedProducts".</p>
<p> We will use existing method "searchProducts" as a boiler plate. We will adjust it to use new parameter for filtering only discontinued products.</p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>class myController extends BaseController
{
    public searchDiscontinuedProducts() {
        
    }
}</code></pre>
</td>
</tr>
<tr>
<td><div class="content-wrapper">
<h3 id="Search-Cookbook-Implementationfortheactionincontroller">Implementation for the action in controller</h3>
<ol>
<li>Inject the search service to our controller.</li>
<li>Create a new object EshopQuery. This object will have all the parameters of our new Solr query.<br />
EshopQuery supports different filters/conditions, which are described in docment below.<br />
For our purpose of getting discontinued products we will use SearchTermCondition, which have the search phrase and the specific field.<br />
In this case, in our current data model, all discontinued fields have the word 'DECLINE' in Solr field ses_product_ses_discontinued_value_s.</li>
<li>Add a condition for visibility, to make sure only visible products are returned.</li>
<li>Add boosting that sku and name is more important than long description.
<ol>
<li>
<p>The field boosting condition is unfortunately not implementation independent. That means, that the field name values, which are passed to the constructor, are different for Econtent and eZ Content search.</p>

</li>
<li>For eZ Content, you need to pass the content field identifier strings.</li>
<li>For Econtent you need to pass the raw Solr field names, including the default search field (which is "text" by default).</li>
</ol></li>
<li>Additionally add offset and limit for pagination purposes.</li>
<li>Set sorting by relevance. For more option see document below.</li>
<li>Add product facet option for manufacturers. For more option see document below.</li>
<li>Now we perform the search with the new parameters.</li>
<li>The number of total hits will be available in numFound property.</li>
<li>The search results will be available in array resultLines.</li>
<li>Facets for search result will be available in array facets.</li>
</ol>
<p><br />
</p>
<p><br />
</p>
</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>class myController extends BaseController
{
    public searchDiscontinuedProductsAction() {
        
        // 1. First we inject the search and facet service
        $searchService = $this-&gt;get(&#39;siso_search.ezsolr_search_service&#39;);
 
        // 2. Now we create the query with all the parameters we want.
        $eshopQuery = new EshopQuery();
        $eshopQuery-&gt;addCondition(new SearchTermCondition(array(
            &#39;searchTerm&#39; =&gt; &#39;DECLINE&#39;,
            &#39;fieldRestrictions&#39; =&gt; &#39;ses_product_ses_discontinued_value_s&#39;
        )));
 
        // 3. Add visibility
        $eshopQuery-&gt;addCondition(new VisibilityCondition(array(&#39;visibility&#39; =&gt; 0)));
 
        // 4.b Add boosting (eZ Content)
        $boosts = array(
            &#39;ses_name&#39; =&gt; 50,
            &#39;ses_sku&#39; =&gt; 100
            &#39;ses_intro&#39; =&gt; 20
            &#39;ses_short_description&#39; =&gt; 2
            &#39;ses_long_description&#39; =&gt; 1.5
            &#39;ses_manufacturer&#39; =&gt; 2
        );
        // 4.c Add boosting (Econtent)
        $boosts = array(
            &#39;text&#39; =&gt; 1,
            &#39;ses_product_ses_name_value_t&#39; =&gt; 50,
            &#39;ses_product_ses_sku_value_s&#39; =&gt; 100
            &#39;ses_product_ses_intro_value_t&#39; =&gt; 20
            &#39;ses_product_ses_short_description_value_t&#39; =&gt; 2
            &#39;ses_product_long_description_value_t&#39; =&gt; 1.5
            &#39;ses_product_ses_manufacturer_value_s&#39; =&gt; 2
        );
        $eshopQuery-&gt;addCondition(new FieldBoosting(array(&#39;boost&#39; =&gt; $boosts)));
 
        // 5. Add offset and limit.
        $eshopQuery-&gt;setOffset(0);
        $eshopQuery-&gt;setLimit(10);
 
        // 6. Add sorting option by relevance
        $eshopQuery-&gt;setSortCriteria(array(new RelevanceSorting()));
 
        // 7. Add product facet option
        $productFacet = new SimpleProductFieldFacet(
            array(
                &#39;fieldName&#39; =&gt; &#39;manufacturer&#39;,
                &#39;labelKey&#39; =&gt; &#39;manufacturer&#39;,
                &#39;filterType&#39; =&gt; &#39;multi_or&#39;,
                &#39;sortType&#39; =&gt; &#39;alpha&#39;,
            )
        );
        $eshopQuery-&gt;addFacet($productFacet);
       
        // 8. Finally we perform the search.
        $productSearchResult = $searchService-&gt;searchProducts($eshopQuery, new SearchContext());
 
        // 9. The hits could be found here 
        $numberOfHits = $productSearchResult-&gt;numFound;
 
        // 10. The search results can be found in this array:
        $productSearchResult-&gt;resultLines
 
        // 11. The search result facets can be found in this array:
        $productSearchResult-&gt;facets
    }
}</code></pre>
</td>
</tr>
<tr>
<td><h3 id="Search-Cookbook-ConditionsAPI">Conditions API</h3></td>
<td><div class="content-wrapper">
<p><strong>ContentTypesCondition</strong>: filter results by content type. Valid content types are ses_product, ses_category or ses_content among others.</p>
<p><strong>SearchTermCondition</strong>: filter results by phrase, also a specific field could be set. Example: search for SE10000 in field sku. That would look like this:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>$myQuery-&gt;addCondition(new SearchTermCondition(array(
    &#39;searchTerm&#39; =&gt; &#39;SE10000&#39;,
    &#39;fieldRestrictions&#39; =&gt; &#39;ses_product_ses_sku_value_s&#39;
)));</code></pre>
<p><strong>SectionCondition</strong>: filter results by section id. Returns only elements that belong to given section id.</p>
<p><strong>SubtreeCondition</strong>: filter results by setting a path. Only under this path elements will be fetched.</p>
<p><strong>VisibilityCondition</strong>: filter results that are shown or hidden.</p>
</td>
</tr>
<tr>
<td><h3 id="Search-Cookbook-BoostingAPI">Boosting API</h3></td>
<td><p><strong>FieldBoosting</strong>: defines which fields should be boosted in search result. This means they will have higher priority in search.</p></td>
</tr>
<tr>
<td><h3 id="Search-Cookbook-SortingAPI">Sorting API</h3></td>
<td><p><strong>RelevanceSorting</strong>: sorts the results by relevance, which is internal solr implementation for "score" field.</p>
<p><strong>ProductFieldSorting</strong>: sorting for products, which supports sorting by name, sku or price in given direction.</p>
<p><strong>ContentNameSorting</strong>: sorting by eZ field name in given direction.</p></td>
</tr>
<tr>
<td><h3 id="Search-Cookbook-FacetsAPI">Facets API</h3></td>
<td><p><strong>SimpleProductFieldFacet</strong>: facet for specific product field.</p>
<p><strong>ProductSpecificationsFieldFacet</strong>: custom facet for product specification information, which is a complex eZ matrix type.</p></td>
</tr>
</tbody>
</table>

# How to configure new tab in search result

In order to show new tab in the search result page we need to add configuration for new group.

Example: new "**download**" tab, which contains content information

``` 
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

# How to implement custom condition for search

If in the project we would need some custom condition we need to implement some classes:

## Steps

<table>
<tbody>
<tr>
<td><ol>
<li>Create new class that implements ConditionInterface</li>
</ol></td>
<td><div class="content-wrapper">
<p>We need to implement a ValueObject which checks if all necessary information for this condition are set</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>class CustomCondition extends ValueObject implements ConditionInterface
{
    /**
     * @var null|array $checkProperties
     */
    protected $checkProperties = array(
        array(&#39;name&#39; =&gt; &#39;customString&#39;, &#39;mandatory&#39; =&gt; true, &#39;type&#39; =&gt; &#39;string&#39;),
    );

    /**
     * @var string
     */
    protected $customString;
}</code></pre>
</td>
</tr>
<tr>
<td>2. Create new handler class</td>
<td><div class="content-wrapper">
<p>Any of the query criteria from eZ Platform can be used here instead "Query\Criterion\Visibility"</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>class CustomConditionHandler implements EzSearchClauseHandlerInterface
{
    /**
     * @param SearchClauseInterface $searchClause
     * @param Query $query
     */
    public function handleSearchClause(SearchClauseInterface $searchClause, Query $query)
    {
        if (!($query-&gt;query instanceof Query\Criterion\LogicalOperator)) {
            return;
        }
        $ezCriterion = new Query\Criterion\Visibility($searchClause-&gt;customString);
        $query-&gt;query-&gt;criteria[] = $ezCriterion;
    }

    /**
     * @param SearchClauseInterface $searchClause
     * @return bool
     */
    public function canHandle(SearchClauseInterface $searchClause)
    {
        return $searchClause instanceof CustomCondition;
    }
}</code></pre>
</td>
</tr>
<tr>
<td> 3. Add configuration in services.xml </td>
<td><div class="content-wrapper">
<p>This will register handler and it will be used whenever search clause is an instance of CustomCondition.</p>
<p>This means that when you add this to eshopQuery it will executre the handler</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&lt;parameter key=&quot;siso_search.search_clause_handler.custom.ezsolr.class&quot;&gt;path\to\CustomConditionHandler&lt;/parameter&gt;

&lt;service id=&quot;siso_search.search_clause_handler.custom.ezsolr&quot; class=&quot;%siso_search.search_clause_handler.custom.ezsolr.class%&quot;&gt;
    &lt;tag name=&quot;siso_search.search_clause_handler&quot; type=&quot;ezsolr&quot; /&gt;
&lt;/service&gt;</code></pre>
</td>
</tr>
</tbody>
</table>

# How to implement custom sorting option for search

If in the project we would need some custom sorting option we need to implement some classes:

## Steps

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr>
<td><ol>
<li>Create new class that extends AbstractSortCriterion</li>
</ol></td>
<td><div class="content-wrapper">
<p>We need to implement a ValueObject which checks if all necessary information for this condition are set</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>class CustomFieldSorting extends AbstractSortCriterion
{
}</code></pre>
</td>
</tr>
<tr>
<td>2. Create new handler class</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>class CustomFieldSortingHandler implements EzSearchClauseHandlerInterface
{
    /**
     * @param SearchClauseInterface $searchClause
     * @param Query $query
     */
    public function handleSearchClause(SearchClauseInterface $searchClause, Query $query)
    {
        $field = new Field(&#39;ses_custom_field&#39;, &#39;ext_ses_custom_field_f&#39;);
        $query-&gt;sortClauses[] = $field;
    }

    /**
     * @param SearchClauseInterface $searchClause
     * @return bool
     */
    public function canHandle(SearchClauseInterface $searchClause)
    {
        return $searchClause instanceof CustomFieldSorting;
    }
}</code></pre>
</td>
</tr>
<tr>
<td> 3. Add configuration in services.xml</td>
<td><div class="content-wrapper">
<p>This will register handler and it will be used whenever search clause is an instance of CustomFieldSorting.</p>
<p>This means that when you add this to eshopQuery it will executre the handler</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&lt;parameter key=&quot;siso_search.search_clause_handler.custom_sort.ezsolr.class&quot;&gt;path\to\CustomFieldSorting&lt;/parameter&gt;

&lt;service id=&quot;siso_search.search_sort_handler.custom_sort.ezsolr&quot; class=&quot;%siso_search.search_sort_handler.custom_sort.ezsolr.class%&quot;&gt;
    &lt;tag name=&quot;siso_search.search_clause_handler&quot; type=&quot;ezsolr&quot; /&gt;
&lt;/service&gt;</code></pre>
</td>
</tr>
</tbody>
</table>

### Please note that a query could have several sorting criteria

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><div class="container" title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<var><code class="sourceCode php"><span class="kw">$eshopQuery</code></var><code class="sourceCode php">-&gt;setSortCriteria<span class="ot">(</code>

<div class="line number2 index1 alt1">
<code class="sourceCode php">    </code><code class="sourceCode php"><span class="kw">array</code><code class="sourceCode php"><span class="ot">(</code>

<div class="line number3 index2 alt2">
<code class="sourceCode php">        </code><code class="sourceCode php"><span class="kw">new</code> <code class="sourceCode php">MyCustomSorting1<span class="ot">(</code><code class="sourceCode php"><span class="kw">array</code><code class="sourceCode php"><span class="ot">(</code><code class="sourceCode php"><span class="st">&#39;direction&#39;</code> <code class="sourceCode php">=&gt; </code><code class="sourceCode php"><span class="st">&#39;asc&#39;</code><code class="sourceCode php"><span class="ot">)),</code>

<div class="line number4 index3 alt1">
<code class="sourceCode php">        </code><code class="sourceCode php"><span class="kw">new</code> <code class="sourceCode php">MyCustomSorting2<span class="ot">(</code><code class="sourceCode php"><span class="kw">array</code><code class="sourceCode php"><span class="ot">(</code><code class="sourceCode php"><span class="st">&#39;direction&#39;</code> <code class="sourceCode php">=&gt; </code><code class="sourceCode php"><span class="st">&#39;desc&#39;</code><code class="sourceCode php"><span class="ot">)),</code>

<div class="line number5 index4 alt2">
<code class="sourceCode php">        </code><code class="sourceCode php"><span class="kw">new</code> <code class="sourceCode php">MyCustomSorting3<span class="ot">(</code><code class="sourceCode php"><span class="kw">array</code><code class="sourceCode php"><span class="ot">(</code><code class="sourceCode php"><span class="st">&#39;direction&#39;</code> <code class="sourceCode php">=&gt; </code><code class="sourceCode php"><span class="st">&#39;desc&#39;</code><code class="sourceCode php"><span class="ot">)),</code>

<div class="line number6 index5 alt1">
<code class="sourceCode php">        </code><code class="sourceCode php"><span class="st">...</code>

<div class="line number7 index6 alt2">
<code class="sourceCode php">    </code><code class="sourceCode php"><span class="ot">)</code>

</td>
</tr>
</tbody>
</table>

The order of the sorting criteria is important. In the example above the sorting will be in the order they are set into the array.

# How to implement an indexer plugin for custom field types

The indexer is a class that implements an interface from "ezplatform-solr-search-engine" called DocumentMapperPluginInterface.

This allows additional data to be indexed in solr. For example to be able to perform search query that as a condition has this additional complex data type like specification matrix, variant matrix, or any other field.

Interface have only two methods that needs to be implemented:

  - this method defines which content type is handled by the indexer plugin.

    ``` 
    public function canExtend(Type $type);
    ```

  - this method is called for every indexed content object and language. The output is an array of SPI Fields objects which are used by the main indexer to create Solr fields.

    ``` 
    public function createExtensionFields(Content $content, Type $type, $languageCode);
    ```

The generated Field instances have a prefix "**ext\_**" in their names to avoid naming conflicts with existing content fields and to distinguish them from standard fields.

## Steps

<table>
<tbody>
<tr>
<td><p>1. Create a class that extends DocumentMapperPluginInterface and define a service for that class.</p></td>
<td><div class="content-wrapper">
<p>Example:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>class MyCustomMapperPlugin implements DocumentMapperPluginInterface</code></pre>
<p>Make sure your plugin uses this files:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>use eZ\Publish\SPI\Persistence\Content;
use eZ\Publish\SPI\Persistence\Content\Type;
use eZ\Publish\SPI\Search\Field;
use eZ\Publish\SPI\Search\FieldType\FloatField;
use EzSystems\EzPlatformSolrSearchEngine\DocumentMapperPluginInterface;
use Siso\Bundle\SearchBundle\Helper\EzSolrSpecificationsIndexHelper;
use Siso\Bundle\SearchBundle\Helper\StaticDocumentMapperPluginHelper as Helper;</code></pre>
</td>
</tr>
<tr>
<td><p>2. Create a service for your new class.</p></td>
<td><div class="content-wrapper">
<p>Example from the services.yml file inside search bundle:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&lt;parameter key=&quot;siso_search.my_custom_mapper_plugin.class&quot;&gt;Siso\Bundle\SearchBundle\Service\MyCustomMapperPlugin&lt;/parameter&gt;
 
&lt;!-- Start Indexer Plugins --&gt;
&lt;service id=&quot;siso_search.my_custom_mapper_plugin&quot; class=&quot;%siso_search.my_custom_mapper_plugin.class%&quot;&gt;
    &lt;tag name=&quot;ezpublish.search.solr.document_mapper_plugin&quot; /&gt;
&lt;/service&gt;</code></pre>
<p>This will allow your plugin execution every time the index is command is executed.</p>
</td>
</tr>
<tr>
<td><p>3. Define which elements will be handled by your plugin using method canExtend().</p></td>
<td><div class="content-wrapper">
<p>Example:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>public function canExtend(Type $type)
{
    return $type-&gt;identifier === &#39;ses_product&#39;;
}</code></pre>
<p>In this case we want to create additional indexes for ses_product. We could also use ses_category, or any other identifier.</p>
</td>
</tr>
<tr>
<td><p>4. Create any logic you want to extend the indexer. Please note that the input parameters of the main methods are not regular catalog elements but standard EzContent and type.</p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<p>Example:</p>
<p>You will have to use some additional logic to get the field names you need for your custom indexing by implementing:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>public function createExtensionFields(Content $content, Type $type, $languageCode);</code></pre>
<p><br />
</p>
<p>If you know your field identifier you can use the static method getFieldDefinitionId() from the helper class to get the field ids that will be processed.</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>$myFieldId = Helper::getFieldDefinitionId($type, $myFieldIdentifier);</code></pre>
<p>Then you just need to iterate each field over the $content and if the field Id matches the fields you want to process you just add your logic.</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>// Iteration over each field inside the content.
foreach ($content-&gt;fields as $field) {
    // If the field matches the id we want to process we can add our logic.
    if ($field-&gt;fieldDefinitionId === $myFieldId
        &amp;&amp; $field-&gt;languageCode === $languageCode
    ) {
        // Our index logic for example indexing price elements as floats:
        $productPrice = Helper::getFieldValue($content, $myFieldId); // This helper function will retrieve the field value.
        if ($productPrice !== null) {
            $outputFields[] = new Field( // We are returning and array of Fields.
                &#39;ext_ses_product_price&#39;, //this is the name of the Solr index.
                Helper::normalizeFloatString((string) $productPrice), // this will transform the field to float.
                new FloatField()    // This will specify element data type. 
                                        // Valid types are: IntegerField(), FloatField(), 
                                    // StringField(), MultipleIntegerField and MultipleStringField().
            );
        }
    }
}</code></pre>
<p>Please note that we don't yet support multiple float field for the external Solr field.</p>
<p>At the end we will just return the array of fields:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>return $outputFields;</code></pre>
</td>
</tr>
<tr>
<td><p>5. The Solr index should have the name you defined + "_" + the datatype.</p>
<p>We have:</p>
<ul>
<li>"i" for integer, </li>
<li>"s" for string, </li>
<li>"f" for float, </li>
<li>"mi" for multi integer, </li>
<li>"ms" for multi string. </li>
</ul>
<p><br />
We currently don't support multi float.</p>
<p><br />
</p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<p>Example:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>{
    &quot;ext_ses_product_price_f&quot;: 12.5
},</code></pre>
<p><br />
</p>
<p>Multiple strings look like this:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&quot;language_code_ms&quot;: [
    &quot;eng-US&quot;,
    &quot;ger-DE&quot;
],</code></pre>
</td>
</tr>
<tr>
<td><p>6. If you want to test how your new fields looks like in Solr you have to:</p>
<ol>
<li>Run command line indexer.</li>
<li>Perform a Solr query directly in Solr interface.</li>
</ol></td>
<td><div class="content-wrapper">
<p>Example:</p>
<p>This command will create Solr index.</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>php -d memory_limit=-1 bin/console ezplatform:solr_create_index</code></pre>
<p>Make sure you execute this in your project home directory.</p>
<p>To perform a Solr query go to Solr URL: usually:<br />
<a href="http://localhost:8983/solr/" class="external-link">http://localhost:8983/solr/</a></p>
<p>The select your collection (usually collection1) and then select query.</p>
<p> Additionally you can set Solr by only showing your new field by adding your field name to the fl field.</p>
<p><img src="attachments/23560665/23563835.png" class="confluence-embedded-image" /></p>
<p><br />
</p>
</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [Screen Shot 2016-02-08 at 11.30.19.png](attachments/23560665/23563835.png) (image/png)  
