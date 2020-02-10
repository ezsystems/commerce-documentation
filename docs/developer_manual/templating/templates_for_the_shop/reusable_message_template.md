#  Reusable message template 

We use this template to render inline messages within the content pages. With this approach we are more flexible. Changes in one file affects all the places where it's included.

#### File location:

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/parts/message.html.twig
```

## Parameters

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
<th>Required</th>
</tr>
</thead>
<tbody>
<tr>
<td>content</td>
<td>Content that is displayed inside a message box. Can be mixed with any type of HTML / text</td>
<td>Yes</td>
</tr>
<tr>
<td>close</td>
<td><p>Flag - allow to close the message</p>
<p><strong>Default</strong>: false</p>
<p><strong>Accepts</strong>: true or false</p></td>
<td>No</td>
</tr>
<tr>
<td>attrs</td>
<td><p>Array of attributes. It accepts any HTML attributes (that is allowed for a &lt;div&gt; element) as well as some special attributes.</p>
<p><strong>Common attributes</strong>:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&#39;class&#39;: &#39;alert|success|info|warning|secondary&#39;,
&#39;id: &#39;message-id&#39;,
&#39;data-custo-attr&#39;: &#39;data custom value&#39;,
&#39;style&#39;: &#39;display: none;&#39;</code></pre>
<p>Since we use Foundation framework we can pass a CSS class that will style message based on the class we pass. Currently we support these classes:</p>
<ul>
<li>alert (for alerts/errors)</li>
<li>success</li>
<li>info</li>
<li>warning</li>
<li>secondary</li>
</ul>
<p>It's possible to pass multiple class names if required, e.g.:</p>
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>&#39;class&#39;: &#39;alert radius u-margin-top-1x&#39;</code></pre>

</td>
<td>No</td>
</tr>
</tbody>
</table>

## Examples

### Simple success message:

``` 
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': 'hello world',
  'attrs':  {
    'class': 'success'
  } }) }}
```

### Simple alert message with close icon:

``` 
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': 'This is an error message',
  'close': true,
  'attrs':  {
    'class': 'alert'
  } }) }}
```

### Notice message with multiline content:

``` 
// setting the message content
{% set noticeMsg %}
  {% for n in notice %}
    <p>{{ n }}</p>
  {% endfor %}
{% endset %}

{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': noticeMsg, // using the noticeMsg variable
  'attrs':  {
    'class': 'info'
  } }) }}
```

### Warning message with st\_translate() as a content:

``` 
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': 'search_result_empty'|st_translate('search'),
  'attrs':  {
    'class': 'warning'
  } }) }}
```

### Alert message with an extra attribute for behat testing:

``` 
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': 'Your Basket is empty'|st_translate ~ '!',
  'attrs':  {
    'class': 'alert',
    'behat': st_tag('message', 'error', 'exists', '', '')
  } }) }}
```

### Alert message with some extra css classes and inline styling:

``` 
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': "Start date must be lower than end date."|st_translate(),
  'attrs':  {
    'class': 'alert js-order-history-dates-invalid',
    'style': 'display: none;'
  } }) }}
```

### Behat extra information

Message elements will render with data tag followed by the behat attirs parameters separated by a '-'

Example to check for a success message with the following configuration:

``` 
// twig file with success message:
include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': s,
  'close': true,
  'attrs':  {
    'class': 'success',
    'behat': st_tag('message', 'success', 'exists', '', '')
```

``` 
// Behat file FeatureContext.php
// This method will check for existing message of a type specified     
// Given $messageType = 'success'
public function iShouldSeeAMessageOfType($messageType)
    {
        $xpath = sprintf("//*[@data-tag-message-%s-exists]", $messageType);
        return $this->iShouldSeeAnElementWithXpath($xpath);
    }
```
