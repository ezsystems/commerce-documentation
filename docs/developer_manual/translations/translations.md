#  Translations 

# Introduction

eZ Commerce offers the possibility to use objects from eZ Platform as translation objects, called "textmodule". The translation service checks first if an object with a specific identifier exists and return the text attribute of this object. If no object was found the standard symfony translation service will be used. 

For details of symfony translations see also <http://symfony.com/doc/current/book/translation.html>

# Twig filter

The translation service offers the twig filter **st\_translate**. 

It uses a code which identifies the text to be translated and an optional context. The context can be used to differentiate between different meanings, e.g. in context "shop" order means to "buy something" but in the CMS part it may have the meaning of "sorting content".

``` 
{{ 'sku'|st_translate() }}
```

#### Examples of usage

``` 
{# Use twig filter #}
{{  messageOrCode|st_translate }}

{# Use the optional context parameter #}
{{  messageOrCode|st_translate('context') }}

{# Only possible if using Symfony Translations, not Textmodules #}
{# Use a message with placeholders and define a different translation domain: validators.de.xliff #}
<h2>{{ 'This is a test with %placeholder%'|st_translate('', { '%placeholder%':'My text' }, 'validators' ) }}</h2>

#specify trans domain - only possible for symfony translations
{{ 'error'|st_translate(null, {}, 'validators') }}
```

### Specify the translation language

Per default the translation service use the language of the current siteaccess and current locale. If desired it is possible to send this parameter in addition.

The translation service is able to use given siteaccess in order to specify the eZ language or locale required for the translation process.

``` 
{# get siteaccess e.g. from basket #}
{% set siteaccess = basket.dataMap.siteaccess is defined ? basket.dataMap.siteaccess : null %}

{# Translation in another siteaccess than current siteaccess #}
{{ 'Thank you for using our shop.'|st_translate(null, {}, null, siteaccess) }}
```

### Pluralisation

Symfony itself offers a possibility to use pluralisation for translations. So it is easy to handle translations that may have plural based on some variables.

eZ Commerce doesn´t have any custom logic to handle pluralisation. If you need to handle such a translations, please use [Symfony](http://symfony.com/doc/current/components/translation/usage.html#component-translation-pluralization) for that.

**Example**

``` 
<div class="grid_12">
    <h3 class="header">
        {% transchoice basket.lines.count with {'%count%' : basket.lines.count} %}
        {1}%count% item in your basket|%count% items in your basket
        {% endtranschoice %}
    </h3>
 
```

Important\!

If you use the {% transchoice %} filter, please make sure, that you always add the translation to your translation file. Missing translation will throw an exception\!

    return array(
    ...

    '{1}%count% item in your basket|%count% items in your basket' => '1 item in your basket|%count% items in your basket',
    ...

``` 
);
```

## PHP Translation Service

The same like in Twig you can use in PHP via service **silver\_trans.translator**.

``` 
$messageOrCode = 'This is either some message that should be translated or a code for a text module';
$context = 'context';

//Call the service
$container->get('silver_trans.translator')->translate($messageOrCode);

//Use the optional context parameter
$container->get('silver_trans.translator')->translate($messageOrCode, $context);
```

## Translations in eZ Platform via content type textmodules

The content type textmodule has the following attributes:

| Field      | Description                                                      |
| ---------- | ---------------------------------------------------------------- |
| Name       | Used for internal name in backend                                |
| Identifier | Here the source or identifier for translation has to be defined. |
| Context    | Optional context                                                 |
| Content    | Content for frontend                                             |

### Field identifier  
By default the translated value is get from the field 'content'. However it is possible to extend the text module class with a new field from a type *XML block*. If you do so, the translation service is able to fetch from the appropriate field.

The take advantage of this, use parameter 'fieldIdentifier':

``` 
{# without context #}
{{ 'my_profile_intro_text'|st_translate(null, {'fieldIdentifier' : 'my_field_identifier' }) }}

{# with context #}
{{ label_tooltip_description|st_translate ('createrma', {'fieldIdentifier' : 'header'}) }}
```

## Configuration

For translations exist the following configurations:

<table>
<thead>
<tr class="header">
<th>Description</th>
<th>Configuration</th>
<th>Default</th>
</tr>
</thead>
<tbody>
<tr>
<td>LocationID of textmodule folder</td>
<td><pre><code>silver_tools.default.translationFolderId</code></pre></td>
<td>89</td>
</tr>
<tr>
<td>Enable/disable textmodules ("true" or "false")</td>
<td><pre><code>silver_tools.default.textmoduleTranslationEnabled</code></pre></td>
<td>true</td>
</tr>
<tr>
<td>Enable/disable default Symfony translation</td>
<td><pre><code>silver_tools.default.defaultTranslationEnabled</code></pre></td>
<td>true</td>
</tr>
<tr>
<td>Enable/disable logging missing translations</td>
<td><pre><code>silver_tools.default.loggTranslations</code></pre></td>
<td>false</td>
</tr>
<tr>
<td>Enable/disable translation cache</td>
<td><pre><code>silver_tools.default.translation_cache</code></pre></td>
<td>true</td>
</tr>
<tr>
<td><p>Defines how long the translation will be stored in the cache.</p>
<p>When TTL value is set to null, then cache is generated forever.<br />
</p></td>
<td><pre><code>silver_tools.default.translation_cache_ttl</code></pre></td>
<td><br />
</td>
</tr>
</tbody>
</table>

# Logging

Translations that are missing are logged. You can enable/disable logging of missing translations in the configuration (see configuration chapter)

All missing translations are logged to this file:

    var/logs/siso.translations.log

``` 
<service id="silver_trans.logging_handler.stream" class="%monolog.handler.stream.class%">
    <argument type="string">%kernel.logs_dir%/siso.translations.log</argument> <!-- This is the file definition -->
</service>
```

## Cache

Translation is cached using Stash. Currently there are 2 types of translations, which comes from eZ Platform or Symfony.

  - Translations which comes from eZ Platform can be cached using Stash. If there is no translation for the given message (or code) in eZ Platform, it will be cached anyway using special "null" translation message. It allows to avoid repeating fetches from eZ Platform.
  - Translations which comes from Symfony are not cached.

#### Purging of cache

When st\_translate() is used in twig templates the cache will be tagged with content-\<content-id\>. In case a text module is updated in eZ Platform eZ will purge all http cache blocks which are tagged with the given content\_id. In addition a signalslot handler will purge the stash cache for this translation as well.

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-06-19 um 14.11.00.png](attachments/23560284/23563806.png) (image/png)  
