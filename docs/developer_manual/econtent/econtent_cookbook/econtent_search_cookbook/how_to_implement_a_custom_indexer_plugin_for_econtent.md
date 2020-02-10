#  How to implement a custom indexer plugin for econtent 

Creating a plugin for econtent is very easy. In the following example we will create a plugin for econtent that will index an additional field based on a serialised array that comes from the database.

**Input**

  - In database we have some keys and values for articles that come in a serialised format like this:

    ``` 
    ses_datamap_additional_data = a:2:{s:4:"Code";s:3:"AXW";s:4:"Name";s:20:"Awesome Speaker AX50";}
    ```

**Desired Output**

  - Index field additional\_product\_code with value AXW
  - Index field additional\_product\_name with value Awesome Speaker AX50

## What to implement

<table>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Create a plugin class that implements
<pre><code>EcontentIndexerPluginInterface</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: php; gutter: false; theme: DJango" data-theme="DJango"><code>class AdditionalDataIndexerPlugin implements EcontentIndexerPluginInterface
{

    /*
     * It is used as a plugin for econtent indexer.
     *
     */
    public function buildIndexFields(array $econtentData, $elementType)
    {
    }

}</code></pre>
</td>
</tr>
<tr>
<td><p>Create a service definition for this class.</p>
<p>It is very important to add the tag, otherwise the plugin will not be executed.</p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: xml; gutter: false; theme: DJango" data-theme="DJango"><code>&lt;parameter key=&quot;siso_search.additional_data_indexer_plugin.class&quot;&gt;YOURNAMESPACE\AdditionalDataIndexerPlugin&lt;/parameter&gt;
 
&lt;service id=&quot;siso_search.additional_data_indexer_plugin&quot; class=&quot;%siso_indexer.additional_data_indexer_plugin.class%&quot;&gt;
    &lt;tag name=&quot;siso_search.econtent_indexer_plugin&quot; /&gt;
&lt;/service&gt;</code></pre>
</td>
</tr>
<tr>
<td><p>Put your custom logic.</p>
<p>Please note the following.</p>
<ul>
<li>We have to loop through the product data map.</li>
<li>Econtent data map supports 3 types:</li>
<li>strings which are stored in data_text field</li>
<li>integers which are stored in data_int field</li>
<li>floats which are stored in data_float field</li>
</ul>
<p><br />
</p>
<p>For array support we use serialised arrays.</p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: php; gutter: false; theme: DJango" data-theme="DJango"><code>class AdditionalDataIndexerPlugin implements EcontentIndexerPluginInterface
{

    /*
     * It is used as a plugin for econtent indexer.
     *
     */
    public function buildIndexFields(array $econtentData, $elementType)
    {
        // We have to loop for every element in data_map in order to find the desired field name.
        // In this example we are looking for a field named ses_datamap_additional_data
        foreach ($econtentData[&#39;data_map&#39;] as $attribute) {
            if ($attribute[&#39;attribute_name&#39;] === &#39;ses_datamap_additional_data &#39;) {
                // We found our field! Now let&#39;s get the serialised data!
                // Please note that if the stored attribute is a string the value will be in a field name data_text
                $additionalData = unserialize($attribute[&#39;data_text&#39;]);
                if (
                    is_array($additionalData) &amp;&amp;
                    isset($additionalData[&#39;ArrayFieldHash&#39;][&#39;array&#39;][0][&#39;code&#39;]) &amp;&amp;
                    isset($additionalData[&#39;ArrayFieldHash&#39;][&#39;array&#39;][0][&#39;name&#39;])
                ) {
                    return array (
                        &#39;additional_data_code&#39; =&gt; $additionalData[&#39;ArrayFieldHash&#39;][&#39;array&#39;][0][&#39;code&#39;],
                        &#39;additional_data_name&#39; =&gt; trim($additionalData[&#39;ArrayFieldHash&#39;][&#39;array&#39;][0][&#39;name&#39;])
                    );
                }
        }
        return null;
    }
}
 
 </code></pre>
</td>
</tr>
<tr>
<td><h3 id="Howtoimplementacustomindexerpluginforecontent-Results">Results</h3>
<p>This will create two new Solr fields with the following names and values:</p>
<ul>
<li>The prefix (here "ses_product_ses_datamap_ext_") is added automatically and contains the content type (here product) as well</li>
<li>The suffix _s is added because the indexer detects the value as a string.</li>
</ul>
<p>But also supports these additional data types with the corresponding suffix:</p>

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Suffix</th>
<th>Data type</th>
</tr>
</thead>
<tbody>
<tr>
<td>String</td>
<td>_s</td>
</tr>
<tr>
<td>Array (Multiple strings)</td>
<td>_ms</td>
</tr>
<tr>
<td>Float</td>
<td>_f</td>
</tr>
<tr>
<td>Integer</td>
<td><p>_i</p>
<p><br />
</p></td>
</tr>
<tr>
<td>long</td>
<td>_l</td>
</tr>
</tbody>
</table>

<p>The datatype is specified in</p>
<pre><code>EcontentFieldNameResolver</code></pre>
<pre><code></code></pre>
<p><strong>Note about longs:</strong></p>
<p>Please note that Solr max and min integers are the following:</p>
<p>SOLR_MAX_INT = 2147483647;</p>
<pre><code>SOLR_MIN_INT = -2147483648;</code></pre>
<p><br />
</p></td>
<td><pre class="syntax language-json"><code>ses_product_ses_datamap_ext_additional_data_code_s: &quot;AXW&quot;</code></pre>
<pre class="syntax language-json"><code>ses_product_ses_datamap_ext_additional_data_name_s: &quot;Awesome Speaker AX50&quot;</code></pre>
<pre class="syntax language-json"><code></code></pre></td>
</tr>
</tbody>
</table>

If everything works fine, after executing the command line indexer, you should be able to see in Solr this new indexed content.

For instructions about running econtent indexer check this document: [Indexing econtent data](Indexing-econtent-data_23560588.html)
