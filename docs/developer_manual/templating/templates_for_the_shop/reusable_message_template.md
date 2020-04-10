# Reusable message template

We use this template to render inline messages within the content pages. With this approach we are more flexible. Changes in one file affects all the places where it's included.

#### File location:

``` 
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views/parts/message.html.twig
```

## Parameters

|Name|Description|Required|
|--- |--- |--- |
|content|Content that is displayed inside a message box. Can be mixed with any type of HTML / text|Yes|
|close|Flag - allow to close the message</br>Default: false</br>Accepts: true or false|No|
|attrs|Array of attributes. It accepts any HTML attributes (that is allowed for a <div> element) as well as some special attributes.</br>Common attributes:</br>'class': 'alert|success|info|warning|secondary',</br>'id: 'message-id',</br>'data-custo-attr': 'data custom value',</br>'style': 'display: none;'</br>Since we use Foundation framework we can pass a CSS class that will style message based on the class we pass. Currently we support these classes:</br>alert (for alerts/errors)</br>success</br>info</br>warning</br>secondary</br>It's possible to pass multiple class names if required, e.g.:</br>'class': 'alert radius u-margin-top-1x'|No|

## Examples

### Simple success message:

``` html+twig
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': 'hello world',
  'attrs':  {
    'class': 'success'
  } }) }}
```

### Simple alert message with close icon:

``` html+twig
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': 'This is an error message',
  'close': true,
  'attrs':  {
    'class': 'alert'
  } }) }}
```

### Notice message with multiline content:

``` html+twig
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

### Warning message with `st_translate()` as a content:

``` html+twig
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': 'search_result_empty'|st_translate('search'),
  'attrs':  {
    'class': 'warning'
  } }) }}
```

### Alert message with an extra attribute for behat testing:

``` html+twig
{{ include('SilversolutionsEshopBundle:parts:message.html.twig'|st_resolve_template, {
  'content': 'Your Basket is empty'|st_translate ~ '!',
  'attrs':  {
    'class': 'alert',
    'behat': st_tag('message', 'error', 'exists', '', '')
  } }) }}
```

### Alert message with some extra css classes and inline styling:

``` html+twig
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

``` html+twig
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
