#  Step 3 - Display the new fields on the products detail page 

What we want to achieve:

In the last chapter we have added 2 new fields:

  - ses\_is\_download
  - ses\_download\_file

All additional product data is stored in the product dataMap. The dataMap is a flexible attribute, that stores project specific data in an assoziative array. You can access the data from the dataMap any time.

The new fields are stored in the dataMap using the *camelCase* convention.

## Override the standard template

Please copy the standard template to the new bundle:    "src/DemoBundle/Resources/views/Catalog/parts/productData.html.twig"

``` 
cp vendor/silversolutions/silver.e-shop//src/Silversolutions/Bundle/EshopBundle/Resources/views/Catalog/parts/productData.html.twig  app/Resources/views/Catalog/parts/productData.html.twig
```

Add a new block in order to inform the customer if a product is  a digital product:

**Template example**

``` 
{% if catalogElement.dataMap.isDownload is defined and catalogElement.dataMap.isDownload == true %}
    <p class="u-no-margin">
        <i class="fa fa-arrow-circle-o-down" aria-hidden="true"></i>
        <strong>
            This is a digital product
        </strong>
    </p>
{% endif %}
```

### Writting conventions

To make usage of the flexible product data system, you have to follow the writting conventions

<table>
<thead>
<tr class="header">
<th>Usecase</th>
<th>Add new field in eZ</th>
<th>How the data is stored in dataMap</th>
<th>How to access the data in the template</th>
</tr>
</thead>
<tbody>
<tr>
<td>Rule</td>
<td><em>snake_case</em> convention</td>
<td><em>camelCase</em> convention without prefix <strong><em>ses</em></strong></td>
<td>Directly, or using the ses_render_field method</td>
</tr>
<tr>
<td>Example</td>
<td><pre><code>ses_is_download</code></pre></td>
<td><pre><code></code></pre>
<pre><code>isDownload</code></pre>
<pre><code></code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>{% if catalogElement.dataMap.isDownload is defined and catalogElement.dataMap.isDownload == true %}
     Display infos about the download
{% endif %}</code></pre>
</td>
</tr>
</tbody>
</table>

### Supported eZ fields

Following eZ fields are supported and will be stored automatically in the product dataMap. The shop is not using the eZ fields to be stored in the dataMap, but custom fields. The main reason is, that the shop supports a [flexible catalog system](#) for products stored not only in eZ.

<table>
<thead>
<tr class="header">
<th>eZ field</th>
<th>Class</th>
<th>In the dataMap converted to</th>
</tr>
</thead>
<tbody>
<tr>
<td>Text line (ezstring)</td>
<td><pre><code>\eZ\Publish\Core\FieldType\TextLine\Value</code></pre></td>
<td><pre><code>Silversolutions\Bundle\EshopBundle\Content\Fields\TextLineField</code></pre></td>
</tr>
<tr>
<td>Rich text (ezrichtext)</td>
<td><pre><code>\eZ\Publish\Core\FieldType\XmlText\Value</code></pre></td>
<td><pre><code>Silversolutions\Bundle\EshopBundle\Content\Fields\TextBlockField</code></pre></td>
</tr>
<tr>
<td>Image (ezimage)</td>
<td><pre><code>\eZ\Publish\Core\FieldType\Image\Value</code></pre></td>
<td><pre><code>Silversolutions\Bundle\EshopBundle\Content\Fields\ImageField</code></pre></td>
</tr>
<tr>
<td>BInary (ezbinary)</td>
<td><pre><code>\eZ\Publish\Core\FieldType\BinaryFile\Value</code></pre></td>
<td><pre><code>Silversolutions\Bundle\EshopBundle\Content\Fields\FileField</code></pre></td>
</tr>
<tr>
<td>SesSelection (sesselection)</td>
<td><pre><code>\Silversolutions\Bundle\DatatypesBundle\FieldType\SesSelection\Value</code></pre></td>
<td><pre><code>Silversolutions\Bundle\EshopBundle\Content\Fields\TextLineField</code></pre></td>
</tr>
</tbody>
</table>

If you want to use another eZ fields (e.g. Date and Time), you would have to override the catalog factory. This topic is handled in the training for <span class="status-macro aui-lozenge aui-lozenge-complete conf-macro output-inline">EZCOMMERCE ADVANCED
