#  Extended reveal modal 

In order to make it easy to use we have created and abstraction layer on top of reveal modal from Foundation (<http://foundation.zurb.com/sites/docs/v/5.5.3/components/reveal.html>). 

## JavaScript:

**File location**

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/js/phalanx/storm.phalanx.utils.js
```

Phalanx utility file contains couple of helpers. Reveal is one of them.

### Available methods:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Name</th>
<th>Description</th>
<th>Examples</th>
</tr>
</thead>
<tbody>
<tr>
<td>storm.reveal.open(id, content, type, size)</td>
<td><p>Opens the modal window using Foundation Reveal Modal. It takes four parameters:</p>
<ul>
<li>id - modal window ID (must be unique)</li>
<li>content - content that is displayed inside a modal</li>
<li>type - type of modal window e.g: alert, success, info, warning</li>
<li>size - size of the modal window (tiny, small, medium, large, xlarge, full). Default is medium</li>
</ul></td>
<td><p><strong>Base example</strong></p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>var content = &#39;html content&#39;;
storm.reveal.open(&#39;my-modal-window&#39;, content, &#39;&#39;);</code></pre>
<p>It will open standard modal window, medium sized.</p>
<p><strong>Large modal window</strong></p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" data-theme="Confluence"><code>var content = &#39;html content&#39;;
storm.reveal.open(&#39;my-modal-window&#39;, content, &#39;&#39;, &#39;large&#39;);</code></pre>

</td>
</tr>
<tr>
<td>storm.reveal.destroy(id)</td>
<td><p>Destroys a modal window. I takes one parameter:</p>
<ul>
<li>id - modal window ID</li>
</ul></td>
<td>storm.reveal.destroy('my-modal-window');</td>
</tr>
</tbody>
</table>

## Sass:

**File location**

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/public/scss/storm/_extend.components.reveal.scss
```

We extend the default Foundation component by adding some extra styling on top of it. With our extended version it's possible to make the modal window look like alert message (with coloured background for success, error, info or warning).

If you want to change/extend it please copy it to your project location.
