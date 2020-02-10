#  Quickorder - API 

Quickorder uses Solr to search for products and variants.

## Configuration

``` 
parameters:
    siso_quickorder.default.autosuggest_delimiters: [' ', '/', '-', '::']

    siso_quickorder.default.csv_delimiters: [';', ',']

    #this delimiter must be part of the siso_quickorder.default.autosuggest_delimiters
    #this delimiter can not be a part of sku or variant code!!!
    siso_quickorder.default.sku_variant_code_delimiter: '::'

    # Columns of the CSV file.
    # The order is important, as it reflects the order in the file.
    siso_quickorder.default.csv_data:
        - sku
        - variantCode
        - quantity
        - customText
```

## Technical background - how variants and products are indexed in Solr

The autosuggest uses the SearchService in order to fetchs suggestions for products.

It uses a configuration which defines which fields shall be considered while searching:

``` 
 siso_solr.default.search.auto_suggest_fields:
        - attr_ses_sku_s
        - attr_ses_name_t
        - ses_variant_list_s
        - ses_variant_codes____s
        - ses_variant_sku____s
        - ses_variant_desc____s
        - ses_variant_main_sku_s
```

The fields ses\_variant\_\* are additional fields indexed in the Solr index. The standard shop is able to index the variant fields stored in eZ Platform Matrix field.

For projects, which extend the product's eZ content type by new fields, the indexer has to be extended, as well:

``` 
"ses_variant_codes____s": [
          "var-wht",
          "var-blk"
        ],
        "ses_variant_sku____s": [
          "se0101",
          "se0101"
        ],
        "ses_variant_desc____s": [
          "xi-5",
          "white",
          "xi-5",
          "black"
        ],
        "ses_variant_list_s": "[{\"sku\":\"SE0101\",\"variantCode\":\"VAR-WHT\",\"description\":\"XI-5 white\",\"characteristicCodeColor\":\"Wei\\u00df\",\"characteristicLabelColor\":\"Wei\\u00df\",\"characteristicCodeUnit\":\"\",\"characteristicLabelUnit\":\"\",\"priceNet\":\"149,00\",\"vatPercent\":\"19\",\"dataMap_countryOfOrigin\":\"\",\"characteristicCodeWidth\":\"\",\"characteristicLabelWidth\":\"\"},{\"sku\":\"SE0101\",\"variantCode\":\"VAR-BLK\",\"description\":\"XI-5 black\",\"characteristicCodeColor\":\"Schwarz\",\"characteristicLabelColor\":\"Schwarz\",\"characteristicCodeUnit\":\"\",\"characteristicLabelUnit\":\"\",\"priceNet\":\"149,00\",\"vatPercent\":\"19\",\"dataMap_countryOfOrigin\":\"\",\"characteristicCodeWidth\":\"\",\"characteristicLabelWidth\":\"\"}]",
        "ses_variant_main_sku_s": "se0101",
```

<table>
<thead>
<tr class="header">
<th>Solr-Attribute</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>ses_variant_codes____s</code></pre></td>
<td>a list of variant codes used for this product</td>
<td><pre><code>&quot;var-wht&quot;,
&quot;var-blk&quot;</code></pre></td>
</tr>
<tr>
<td><pre><code>ses_variant_sku____s</code></pre></td>
<td>a list of corresponding skus</td>
<td><pre><code>&quot;se0101&quot;,
&quot;se0101&quot;</code></pre></td>
</tr>
<tr>
<td><pre><code>ses_variant_desc____s</code></pre></td>
<td>a list of words in lower case used for the description of variants</td>
<td><pre><code>&quot;xi-5&quot;,
&quot;white&quot;,
&quot;xi-5&quot;,
&quot;black&quot;</code></pre></td>
</tr>
<tr>
<td><pre><code>ses_variant_list_s</code></pre></td>
<td>a json version of the variants using these fields</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>Array
(
    [0] =&gt; stdClass Object
        (
            [sku] =&gt; SE0101
            [variantCode] =&gt; VAR-WHT
            [description] =&gt; XI-5 white
            [characteristicCodeColor] =&gt; Weiu00df
            [characteristicLabelColor] =&gt; Weiu00df
            [characteristicCodeUnit] =&gt;
            [characteristicLabelUnit] =&gt;
            [priceNet] =&gt; 149,00
            [vatPercent] =&gt; 19
            [dataMap_countryOfOrigin] =&gt;
            [characteristicCodeWidth] =&gt;
            [characteristicLabelWidth] =&gt;
        )
    [1] =&gt; stdClass Object
        (
            [sku] =&gt; SE0101
            [variantCode] =&gt; VAR-BLK
            [description] =&gt; XI-5 black
            [characteristicCodeColor] =&gt; Schwarz
            [characteristicLabelColor] =&gt; Schwarz
            [characteristicCodeUnit] =&gt;
            [characteristicLabelUnit] =&gt;
            [priceNet] =&gt; 149,00
            [vatPercent] =&gt; 19
            [dataMap_countryOfOrigin] =&gt;
            [characteristicCodeWidth] =&gt;
            [characteristicLabelWidth] =&gt;
        )
</code></pre>
</td>
</tr>
<tr>
<td><pre><code>ses_variant_main_sku_s</code></pre></td>
<td>The sku of the main product</td>
<td><pre><code>se0101</code></pre></td>
</tr>
</tbody>
</table>
