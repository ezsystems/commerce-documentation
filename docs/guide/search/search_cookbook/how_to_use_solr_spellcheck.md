# How to use Solr spellcheck (aka "Did you mean?" functionality)

## Currently this functionality works only if you are using econtent data provider.

### Step 1: Make sure your Solr core is configured correctly

Restart Solr if you did some changes.

Look for solrconfig.xml file inside your solr core directory:

`/solr-4.10.4/example/solr/econtent1/conf/solrconfig.xml`

We should add this lines to the requestHandler named "/select"

Search for:

`<requestHandler name="/select" class="solr.SearchHandler">`

And then add this definitions after:

```
<arr name="last-components">
  <str>spellcheck</str>
</arr>
```

Make sure you have also this element in the xml file:

`<searchComponent name="spellcheck" class="solr.SpellCheckComponent">`

Additional definitions regarding spellcheck can be configured here, but with the default values it should work.

Go to your Solr installation directory and execute the following commands:

``` bash
solr-4.10.4/bin/solr stop -all
solr-4.10.4/bin/solr start
```

### Step 2: Enable Spellcheck in configuration

Modify econtent_search.yml found in search bundle.

`harmony_dev/vendor/silversolutions/silver.e-shop/src/Siso/Bundle/SearchBundle/Resources/config/econtent_search.yml`

Use this configuration:

`siso_search.default.solr_spellcheck: true`

### Step 3: Search controller behavior

The search engine will return Search Collation and Search Term Suggestions.

```
return array(
    $catalogElements,
    $facetGroups,
    $productSearchResult->numFound,
    $productSearchResult->spellcheckCollation,
    $spellcheckCollationResults,
    $productSearchResult->spellcheckSuggestedTerms
);
```

|||
|--- |--- |
|$productSearchResult->spellcheckCollation|(string) This will have the collation text.</br>This is the suggested phrase (should have a complete phrase if the user searched for more than one term.</br>Example: User search for "blac spealer" the collation could be: "black speaker"</br>Note: Collation will have text only if it results are not 0.|
|$spellcheckCollationResults|This will have the amount of hits of collation search.|
|$productSearchResult->spellcheckSuggestedTerms|This will have an array of suggested words and their hits.</br>Example:</br>black => 99</br>spealer => 65</br>It is sorted by amount of hits desc.|

### Step 4

You can check the twig file that display these results:

``` html+twig
{# Start Spellcheck results #}
{% if spellcheckCollationResults is defined and spellcheckCollationResults > 0 %}
  <br/>{{ 'msg.did_you_mean'|st_translate }}
  <a href="{{ path('siso_global_search') }}?query={{ spellcheckCollation }}">{{ spellcheckCollation }}
    (
    {% transchoice spellcheckCollationResults with {'%count%' : spellcheckCollationResults} %}
    {0} No result|[2,Inf[ %count% results|{1} One result
    {% endtranschoice %}
    )
  </a>?
{% endif %}
{% if spellcheckSuggestions is defined and spellcheckSuggestions|length > 0 %}
  <ul>{{ 'msg.suggested_words'|st_translate}}:
    {% for word, frequency in spellcheckSuggestions %}
      <li><a href="{{ path('siso_global_search') }}?query={{ word }}">{{ word }}: {{ frequency }}</a></li>
    {% endfor %}
  </ul>
{% endif %}
{# End Spellcheck results #}
```
