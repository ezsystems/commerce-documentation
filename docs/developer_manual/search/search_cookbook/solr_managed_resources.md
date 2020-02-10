#  Solr Managed Resources 

Work in progress.

## Method 1: Creating a managed synonym resource defined in schema.xml

With this method we will create a managed synonym resource that can be updated via REST.

``` 
<!-- A text type for English text where stopwords and synonyms are managed using the REST API -->
<fieldType name="managed_en" class="solr.TextField" positionIncrementGap="100">
  <analyzer>
    <tokenizer class="solr.StandardTokenizerFactory"/>
    <filter class="solr.ManagedSynonymFilterFactory" managed="english" />
  </analyzer>
</fieldType>
```

This definition should be putted in schema.xml files. It will define solr.ManagedSynonymFilterFactory for synonyms main class instead of SynonymFilterFactory

Now we have to add the dynamic field definitions for the fields that we want to use synonyms.

``` 
<dynamicField name="*_s"  type="managed_en"    indexed="true"  stored="true" multiValued="true"/>
```

This code should replace the current definition of this dynamic field:

\<\!--     \<dynamicField name="\*\_s" type="string" indexed="true" stored="true"/\> --\>

or it may me defined with another name

    <dynamicField name="*value_s"  type="managed_en"    indexed="true"  stored="true" multiValued="true"/>

If we omit this the synonym definition will be taken from file language-fielddefinition.xml where you can view this code:

``` 
    <fieldType name="text" class="solr.TextField" positionIncrementGap="100">
      <analyzer type="index">
        <tokenizer class="solr.StandardTokenizerFactory"/>
        <filter class="solr.StopFilterFactory" ignoreCase="true" words="stopwords.txt" enablePositionIncrements="true" />
        <filter class="solr.LowerCaseFilterFactory"/>
      </analyzer>
      <analyzer type="query">
        <tokenizer class="solr.StandardTokenizerFactory"/>
        <filter class="solr.StopFilterFactory" ignoreCase="true" words="stopwords.txt" enablePositionIncrements="true" />
        <filter class="solr.SynonymFilterFactory" synonyms="synonyms.txt" ignoreCase="true" expand="true"/>
        <filter class="solr.LowerCaseFilterFactory"/>
      </analyzer>
    </fieldType>
```

To add a new synonym we just use this curl call:

curl -XPUT "<http://localhost:8983/solr/collection1/schema/analysis/synonyms/english>" -H 'Content-type:application/json' --data-binary '{"word":\["synonym1","synonym2"\]}'

Please note that with current version of Solr, if we use managed synonyms we have to stick to one to many synonyms definition. In the example above the a search for word, or synonym1, or synonym2 will match any records containing word but a search for word or synonym1 will never return results with the words synonym1 or synonym2.

If you decide to use the synonym file it is possible to define synonyms of the same level. As a result, any synonym will match any other in the search result.

// Create synonyms REST  
// curl -XPUT -H 'Content-type:application/json' --data-binary '{"class":"org.apache.solr.rest.schema.analysis.ManagedSynonymFilterFactory$SynonymManager"}' "<http://localhost:8983/solr/collection1/schema/analysis/synonyms/german>"

// Put value to synonyms  
// curl -X PUT -H 'Content-type:application/json' --data-binary '{"mad":\["angry","upset"\]}' "<http://localhost:8983/solr/collection1/schema/analysis/synonyms/german>"

// Delete all german synonyms  
// curl -XDELETE "<http://localhost:8983/solr/collection1/schema/analysis/synonyms/german>"

// List all german synonyms  
// curl '<http://localhost:8983/solr/collection1/schema/analysis/synonyms/german>'

// Delete a single element  
// curl -X DELETE "<http://localhost:8983/solr/collection1/schema/analysis/synonyms/german/mad>"

/\*  
\*

Managed resources expose a REST API endpoint for performing Create-Read-Update-Delete (CRUD) operations on a Solr object. Any long-lived Solr object that has configuration settings and/or data is a good candidate to be a managed resource.  Managed resources complement other programmatically manageable components in Solr, such as the RESTful schema API to add fields to a managed schema. Consider a Web-based UI that offers Solr-as-a-Service where users need to configure a set of stop words and synonym mappings as part of an initial setup process for their search application. This type of use case can easily be supported using the Managed Stop Filter & Managed Synonym Filter Factories provided by Solr, via the Managed resources REST API. 

Create managed resource using schema xml for stop words and for synonyms

\<fieldType name="managed\_en" class="solr.TextField" positionIncrementGap="100"\>  
\<analyzer\>  
\<tokenizer class="solr.StandardTokenizerFactory"/\>  
\<filter class="solr.ManagedStopFilterFactory" managed="english" /\>  
\<filter class="solr.ManagedSynonymFilterFactory" managed="english" /\>  
\</analyzer\>  
\</fieldType\>

Create using REST API

curl -XPUT -H 'Content-type:application/json' --data-binary '{"class":"org.apache.solr.rest.schema.analysis.ManagedWordSetResource"}' "<http://localhost:8983/solr/collection1/schema/analysis/stopwords/german>"

    curl -XPUT "http://localhost:8983/solr/collection1/schema/analysis/stopwords/german" -H 'Content-type:application/json' --data-binary '["foo"]'

curl "<http://localhost:8983/solr/collection1/schema/managed>"

curl -XDELETE "<http://localhost:8983/solr/collection1/schema/analysis/stopwords/german>"
