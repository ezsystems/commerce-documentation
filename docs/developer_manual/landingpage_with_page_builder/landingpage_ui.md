#  Landingpage - UI 

The prepared landingpage blocks are using templates which can be overridden in a project.

<table>
<thead>
<tr class="header">
<th>Block</th>
<th>Used template</th>
<th>Used sub templates</th>
</tr>
</thead>
<tbody>
<tr>
<td><p>Last viewed items</p></td>
<td>SisoEzStudioBundle:blocks:product_last_viewed_slider.html.twig</td>
<td><p>Uses a Subcontroller SilversolutionsEshopBundle:EzFlow:showLastViewedProducts and the template:</p>
<ul>
<li>SilversolutionsEshopBundle:Catalog:last_viewed_slider.html.twig<br />
</li>
</ul></td>
</tr>
<tr>
<td>Bestsellers</td>
<td>SisoEzStudioBundle:blocks:bestseller.html.twig</td>
<td><p>User a subcontroller SilversolutionsEshopBundle:Bestsellers:getBestsellers and the template:</p>
<ul>
<li>SilversolutionsEshopBundle:Bestsellers:bestsellers_box.html.twig</li>
</ul></td>
</tr>
<tr>
<td>Product slider</td>
<td>SisoEzStudioBundle:blocks:product_slider.html.twig</td>
<td><p>Uses a Subcontroller SilversolutionsEshopBundle:EzFlow:getSkuListByString' and the template:</p>
<ul>
<li>SisoEzStudioBundle:blocks:product_slider_tabs.html.twig</li>
</ul></td>
</tr>
</tbody>
</table>
