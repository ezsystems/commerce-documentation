# Search Solr schema configuration

silver.eShop is using an adapted schema definition which is installed using the shell script install-solr.sh.

## Changes for silver.eShop

The following changes are required for the shop. The changes will be applied using the shell script install-solr.sh. 

`custom-fields-types.xml`:

``` xml
<!--
    This generates a field for prefix queries, using edge n-grams.
-->
<field name="ext_prefix_ngram" type="text_edge_ngram" indexed="true" stored="false" required="false" multiValued="true" />
<copyField source="meta_content__text_t" dest="ext_prefix_ngram" />
```

`language-fieldtypes.xml`:

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

In addition the parameter solr.autoSoftCommit.maxTime:20 is set to 20 ms in order to apply changes in Solr in a short time. 
