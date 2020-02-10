#  Econtent dataprovider - How to extend EcontentCatalogFactory class 

Catalog objects are created using EcontentCatalogFactory class. This involves both product and product groups, or other type of elements which could be defined.

Sometimes in projects is needed to extend this class to provide additional functionality or logic to the catalog elements.

Here we will do a step by step extension of EcontentCatalogFactory to add some custom functionality.

Please note that Catalog objects have some default properties + a data map with additional fields. This data map is very flexible and allows a very easy way to add new custom properties to objects. A custom property could anything you want, like a file object, an array of files, images, custom values based on standard values, flags, etc.

In these example we will extend EcontentCatalogFactory to add a video to the product data map.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Steps</th>
<th>Code</th>
</tr>
</thead>
<tbody>
<tr>
<td><p>Step 1: Create the new class that will extend EcontentCatalogFactory</p>
<p>We will use the following name: MyProjectEcontentCatalogFactory</p></td>
<td><p>Create the class file in your project bundle directory.</p>
<strong></strong> Expand source 
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: true; theme: Confluence; collapse: true" data-theme="Confluence"><code>&lt;?php
/
namespace MyProject\Bundle\ProjectBundle\Services\Factory;

class MyProjectEcontentCatalogFactory extends EcontentCatalogFactory
{
 
}</code></pre>

</td>
</tr>
<tr>
<td><p>Step 2: Modify your services.yml file and add the new class you just created.</p>
<p>Please note that if you are not overriding the constructor, it is enough to add only the parameter with the original key and the new class.</p></td>
<td>

<strong></strong> Expand source 
<pre class="" data-syntaxhighlighter-params="brush: php; gutter: true; theme: Confluence; collapse: true" data-theme="Confluence"><code>&lt;parameter key=&quot;silver_catalog.econtent_catalog_factory.class&quot;&gt;MyProject\Bundle\ProjectBundle\Services\Factory\CornelsenEcontentCatalogFactory&lt;/parameter&gt;</code></pre>

</td>
</tr>
<tr>
<td><p>Step 3: Create a new extract method for new video.</p>
<p>In this case we will assume that the object has one video and its name is the sku + mp4 extension.</p>
<p> </p>
<p><strong>Note:</strong></p>
<p>Catalog elements have a data map, which is an array of additional properties. Every new element that is added to the catalog element should be placed in its data map. The catalog element data map supports different Field objects as its elements.</p>
<p>Available Field elements could be found here: EshopBundle/Content/Fields</p>
<p>For this example we will use the Field Field object.</p>
<p> </p>
<p>We also use symphony finder object to support file system access.</p>
<p>For more info on finder you can check:</p>
<p><a href="http://symfony.com/doc/current/components/finder.html" class="external-link">http://symfony.com/doc/current/components/finder.html</a></p></td>
<td>

<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>/**
 * This method will extract all videos of specified extensions.
 *
 * @param $fieldIdentifier
 * @param $dataMap
 * @return FileField
 */
protected function extractVideo($fieldIdentifier, $dataMap)
{
    $attributes = $this-&gt;getFieldFromDataMap($fieldIdentifier, $dataMap);
    $fileName = $attributes[&#39;data_text&#39;].&#39;mp4&#39;;
    $finder = new Finder();
    try {
        $finder-&gt;files()-&gt;in(&#39;videos/&#39;)-&gt;name($fileName);
        /** @var \Symfony\Component\Finder\SplFileInfo $file */
        foreach ($finder as $file) {
            $fileName = $file-&gt;getRelativePathname();
            $fileSize = $file-&gt;getSize();
            $fileRecord = array(
                &#39;fileName&#39; =&gt; $fileName,
                &#39;fileSize&#39; =&gt; $fileSize,
                &#39;path&#39; =&gt; &#39;videos/&#39;.$fileName,
            );
        }
    } catch (\InvalidArgumentException $e) { }
 
    return new FileField($fileRecord);
}</code></pre>

</td>
</tr>
<tr>
<td><p>Step 4. Add the call to extractVideo method in method fillCatalogElementDataMap</p>
<p>Method <strong>fillCatalogElementDataMap</strong> is responsible for generating the data map of the catalog element.</p></td>
<td>Add the call to extractVideo method in method
<pre><code>fillCatalogElementDataMap</code></pre>
<pre><code></code></pre>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>/**
 * Fills the data map of a catalog element
 *
 * @param CatalogElement $catalogElement
 * @param array $dataMap
 * @return CatalogElement
 */
protected function fillCatalogElementDataMap(CatalogElement $catalogElement, array $dataMap = array())
{
. 
.
.
 
    /* Before returning the catalog element we can add the video to data map: */
 
    // The videos will be only for products, so:
    if ($catalogElement instanceof ProductNode) {
        $videoField = $this-&gt;extractVideo(&#39;ses_sku&#39;, $dataMap);
        $catalogElement-&gt;addFieldToDataMap(&#39;video&#39;, $videoField);
    }

    return $catalogElement;
}</code></pre>

</td>
</tr>
<tr>
<td><p>Done!</p>
<p>Your catalog element now should have a video file in its data map.</p>
<p>Keep in mind the following:</p>
<p>data map is an array with different kinds of field objects which are defined here: EshopBundle/Content/Fields</p>
<p>Also arrays of fields can be added. In this case the object will be an ArrayField, and its contents will be Field Objects. You can have an ArrayField with different files fields, text fields or any other kind of defined field.</p>
<p><br />
</p></td>
<td> </td>
</tr>
</tbody>
</table>
