# Solr schema configuration

eZ Commerce uses an adapted schema definition which is installed using the shell script `install-solr.sh`.
The following changes are applied:

In `custom-fields-types.xml`:

``` xml
<!--
    This generates a field for prefix queries, using edge n-grams.
-->
<field name="ext_prefix_ngram" type="text_edge_ngram" indexed="true" stored="false" required="false" multiValued="true" />
<copyField source="meta_content__text_t" dest="ext_prefix_ngram" />
```

In `language-fieldtypes.xml`:

``` xml
<!-- This field can be used to query prefixes up to 15 characters.
         Any term longer than that would be treated as a prefix with only
         the 15 first characters for comparison -->
    <fieldType name="text_edge_ngram" class="solr.TextField" positionIncrementGap="100">
        <analyzer type="index">
            <tokenizer class="solr.LowerCaseTokenizerFactory"/>
            <filter class="solr.EdgeNGramFilterFactory" minGramSize="2" maxGramSize="15"/>
        </analyzer>
        <analyzer type="query">
            <tokenizer class="solr.LowerCaseTokenizerFactory"/>
            <filter class="solr.LengthFilterFactory" min="2" max="15" />
        </analyzer>
    </fieldType>
```

In addition, the `solr.autoSoftCommit.maxTime` parameter is set to 20 ms to apply changes in Solr quickly.
