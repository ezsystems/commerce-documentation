#  How to use Solr spellcheck (aka "Did you mean?" functionality) 

## Currently this functionality works only if you are using econtent data provider.

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
<td><p>Step 1: Make sure your Solr core is configured correctly:</p>
<p> </p>
<p>Restard Solr if you did some changes.</p>
<p> </p></td>
<td><p>Look for solrconfig.xml file inside your solr core directory:</p>
<pre><code>/solr-4.10.4/example/solr/econtent1/conf/solrconfig.xml</code></pre>
<p> </p>
<p>We should add this lines to the requestHandler named "/select"</p>
<p>Search for:</p>
<pre><code>&lt;requestHandler name=&quot;/select&quot; class=&quot;solr.SearchHandler&quot;&gt;</code></pre>
<p>And then add this definitions after:</p>
<div>

<p> </p>

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr>
<td><div class="container" title="Hint: double-click to select code">
<div class="line number1 index0 alt2">
<code class="sourceCode php">&lt;arr name=</code><code class="sourceCode php"><span class="st">&quot;last-components&quot;</code><code class="sourceCode php">&gt;</code>

<div class="line number2 index1 alt1">
<code class="sourceCode php">  </code><code class="sourceCode php">&lt;str&gt;spellcheck&lt;/str&gt;</code>

<div class="line number3 index2 alt2">
<code class="sourceCode php">&lt;/arr&gt;</code>

</td>
</tr>
</tbody>
</table>

<p> </p>
<p>Make sure you have also this element in the xml file:</p>
<pre><code>&lt;searchComponent name=&quot;spellcheck&quot; class=&quot;solr.SpellCheckComponent&quot;&gt;</code></pre>
<p>Additional definitions regarding spellcheck can be configured here, but with the default values it should work.</p>
<p> </p>
<p>Go to your Solr installation directory and execute the following commands:</p>
<pre><code>solr-4.10.4/bin/solr stop -all</code></pre>
<pre><code>solr-4.10.4/bin/solr start</code></pre></td>
</tr>
<tr>
<td><p>Step 2: Enable Spellcheck in configuration:</p></td>
<td><p>Modify econtent_search.yml found in search bundle.</p>
<pre><code>harmony_dev/vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Resources/config/econtent_search.yml</code></pre>
<p>Use this configuration:</p>
<pre><code>siso_search.default.solr_spellcheck: true</code></pre>
<p> </p></td>
</tr>
<tr>
<td> Step 3: Search controller behaviour</td>
<td><p>The search engine will return Search Collation and Search Term Suggestions.</p>
<p> </p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>return array(
    $catalogElements,
    $facetGroups,
    $productSearchResult-&gt;numFound,
    $productSearchResult-&gt;spellcheckCollation,
    $spellcheckCollationResults,
    $productSearchResult-&gt;spellcheckSuggestedTerms
);</code></pre>

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
<td><pre><code>$productSearchResult-&gt;spellcheckCollation </code></pre></td>
<td><p>(string) This will have the collation text.</p>
<p>This is the suggested phrase (should have a complete phrase if the user searched for more than one term.</p>
<p>Example: User search for "blac spealer" the collation could be: "black speaker"</p>
<p>Note: Collation will have text only if it results are not 0.</p>
<p> </p></td>
</tr>
<tr>
<td><pre><code>$spellcheckCollationResults</code></pre></td>
<td><p>This will have the amount of hits of collation search.</p>
<p> </p></td>
</tr>
<tr>
<td><pre><code>$productSearchResult-&gt;spellcheckSuggestedTerms</code></pre></td>
<td><p>This will have an array of suggested words and their hits.</p>
<p>Example:</p>
<p>black =&gt; 99</p>
<p>spealer =&gt; 65</p>
<p>It is sorted by amount of hits desc.</p>
<p> </p></td>
</tr>
</tbody>
</table>

<p> </p>
<p> </p>
<p> </p></td>
</tr>
<tr>
<td>Step 4</td>
<td><p>You can check the twig file that display these results:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{# Start Spellcheck results #}
{% if spellcheckCollationResults is defined and spellcheckCollationResults &gt; 0 %}
  &lt;br/&gt;{{ &#39;msg.did_you_mean&#39;|st_translate }}
  &lt;a href=&quot;{{ path(&#39;siso_global_search&#39;) }}?query={{ spellcheckCollation }}&quot;&gt;{{ spellcheckCollation }}
    (
    {% transchoice spellcheckCollationResults with {&#39;%count%&#39; : spellcheckCollationResults} %}
    {0} No result|[2,Inf[ %count% results|{1} One result
    {% endtranschoice %}
    )
  &lt;/a&gt;?
{% endif %}
{% if spellcheckSuggestions is defined and spellcheckSuggestions|length &gt; 0 %}
  &lt;ul&gt;{{ &#39;msg.suggested_words&#39;|st_translate}}:
    {% for word, frequency in spellcheckSuggestions %}
      &lt;li&gt;&lt;a href=&quot;{{ path(&#39;siso_global_search&#39;) }}?query={{ word }}&quot;&gt;{{ word }}: {{ frequency }}&lt;/a&gt;&lt;/li&gt;
    {% endfor %}
  &lt;/ul&gt;
{% endif %}
{# End Spellcheck results #}</code></pre>

</td>
</tr>
</tbody>
</table>
