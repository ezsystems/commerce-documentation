#  How to adapt the UI for configuration 

## Introduction

Shop administrator is able to configure some of the general parameters in order to customize eZ Commerce for his needs. 

This ability is available after logging to eZ Backend and going into eZ Commerce section.

The backend offers a selection of fields which can be configured. A developer can add new fields if required (see below).  
![](attachments/23560666/23569055.png)  
## How customized parameters are stored

Administrator can update any of the exposed parameters. When button "Save" is clicked there are 2 actions that follow:

  - 1.) The configuration is stored in a database as **eZ Document**. There is a special type called "Configuration", which is storing the yml representation.  
    ![](attachments/23560666/23569049.png)  

  - 2.) A [.yml file](Store-silver.eShop-backend-configuration-in-.yml-file_23560984.html) with this configuration is stored in the file system and afterwards in the stash cache.

## How client configuration is acknowledged by eZ Commerce

In order to apply this additional configuration, which was set by administrator, a new configuration resolver with a high priority has been implemented. This is called before all other config resolvers are called and checks for additional configuration files in the file system.

``` 
<service id="siso_core.config.resolver" class="%siso_core.config.resolver.class%" lazy="true">
    ...
    <tag name="ezpublish.config.resolver" priority="300" />
</service>
```

### How the siso\_core.config.resolver behaves

The siso\_core.config.resolver checks if additional configuration files exists in the file system. Therefore following folder is checked:

``` 
#Attention!!! This parameter is not ment to be changed! It should be only used as readonly parameters!!!
siso_core.default.silver_eshop_config_directory: '%kernel.root_dir%/Resources/silverEshopConfig/'
```

It reads all .yml files stored in this folder and merges them together - as a result one configuration list is created and stored in the stash cache.

It is not the responsibility of this config resolver to check, if the .yml files are valid, or if the content is correct.

Afterwards when the actual configuration parameter is called by the application, it tries to read it from the stash first, otherwise it follows the default behaviour.

## Configuration of parameters exposed to administrators

All the siteaccess parameters can be stored in the backend configuration class. Therefore new parameters have to be added to the backend configuration files.  
These configuration files can be found here: `silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/config/backend`

The parameters are divided into sections by siteaccess and group. 

Example:

``` 
silver_eshop.default.catalog_product_list_limit:
    group: core
    type: integer
```

In the example above the "product limit" is in a default siteaccess (which is shown on the top in a tab) and in core group (which is visible as one of the accordion tables):

**List of parameters available for display:**

<table>
<thead>
<tr class="header">
<th>Type</th>
<th>Options</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>Simple values</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>boolean</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.xxx:
    type: boolean</code></pre>
</td>
<td><p>radio button</p>
<p><br />
</p></td>
<td><div class="content-wrapper">
<img src="plugins/servlet/confluence/placeholder/unknown-attachment" title="FireShot Capture 51 - Silver e-shop - eZ Publ_ - http___admin.harmony.local_ses_legacy_configuration.png" class="confluence-embedded-image" />
</td>
</tr>
<tr>
<td><pre><code>string</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.xxx:
    type: email</code></pre>
</td>
<td>text input field</td>
<td><div class="content-wrapper">
<img src="plugins/servlet/confluence/placeholder/unknown-attachment" title="FireShot Capture 53 - Silver e-shop - eZ Publ_ - http___admin.harmony.local_ses_legacy_configuration.png" class="confluence-embedded-image" />
</td>
</tr>
<tr>
<td><pre><code>email</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.xxx:
    type: email</code></pre>
</td>
<td>text input field</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>integer</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.xxx:
    type: integer</code></pre>
</td>
<td>number input</td>
<td><div class="content-wrapper">
<img src="plugins/servlet/confluence/placeholder/unknown-attachment" title="FireShot Capture 52 - Silver e-shop - eZ Publ_ - http___admin.harmony.local_ses_legacy_configuration.png" class="confluence-embedded-image" />
</td>
</tr>
<tr>
<td><pre><code>numeric</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.xxx:
    type: number</code></pre>
</td>
<td>number input</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>selectbox</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.xxx:
    type: selectbox
    choices: [&#39;choice1&#39;, &#39;choice2&#39;]</code></pre>
</td>
<td>select box with defined choices</td>
<td><div class="content-wrapper">
<img src="plugins/servlet/confluence/placeholder/unknown-attachment" title="Screenshot from 2015-11-10 15:35:40.png" class="confluence-embedded-image" />
</td>
</tr>
<tr>
<td><pre><code>checkbox</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.xxx:
    type: checkbox
    choices: [&#39;choice1&#39;, &#39;choice2&#39;, &#39;choice3&#39;, &#39;choice4&#39;, &#39;choice5&#39;]</code></pre>
</td>
<td>checkboxes with defined choices</td>
<td><div class="content-wrapper">
<img src="plugins/servlet/confluence/placeholder/unknown-attachment" title="FireShot Capture 55 - Silver e-shop - eZ Publ_ - http___admin.harmony.local_ses_legacy_configuration.png" class="confluence-embedded-image" />
</td>
</tr>
<tr>
<td>1 dimensional arrays</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td><pre><code>array</code></pre></td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.xxx:
    type: array
    keys: true|false
    keys_choices: [&#39;choice1&#39;, &#39;choice2&#39;, &#39;choice3&#39;]
    choices : [&#39;choice1&#39;, &#39;choice2&#39;]</code></pre>
</td>
<td><p>inputs or selectboxes with optional choices</p>
<ul>
<li>if keys are allowed then user can specify array keys also</li>
<li>if choice are defined only those values are allowed</li>
</ul>
<p>User interface allows:</p>
<ul>
<li>adding new array items</li>
<li>removing items</li>
</ul></td>
<td><div class="content-wrapper">
<p>Array without keys:</p>
<p><img src="plugins/servlet/confluence/placeholder/unknown-attachment" title="FireShot Capture 75 - Silver e-shop - eZ Publ_ - http___admin.harmony.local_ses_legacy_configuration.png" class="confluence-embedded-image" /></p>
<p>Array with specified keys:</p>
<p><img src="plugins/servlet/confluence/placeholder/unknown-attachment" title="FireShot Capture 74 - Silver e-shop - eZ Publ_ - http___admin.harmony.local_ses_legacy_configuration.png" class="confluence-embedded-image" /></p>
</td>
</tr>
<tr>
<td>2 dimensional arrays</td>
<td><br />
</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>subtype</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>siso_core.default.vat:
    group: core
    type: subtype
    internal_key: DE
    sub_type: array
    sub_keys: true
    sub_choices: [7, 19] </code></pre>
</td>
<td><p>subtype is used for 2 dimensional arrays</p>
<ul>
<li>internal_key - index of parameter in array</li>
<li>sub_xxx - determines options same as array</li>
</ul>
<p><br />
</p></td>
<td>See example above. Sub key is bold.</td>
</tr>
</tbody>
</table>

## Translations

There is a possibility to add translations for configuration name and description that will appear in backend. For subtypes it is allowed to write description per internal key (example below):

``` 
'catalog_product_list_limit|config' => 'Products per page in a product list',
'catalog_product_list_limit|description' => 'This numeric value describes how many products should be visible on category listing page',
// subtype with internal key "active"
'catalog_factory.assets|config' => 'Shall assets (images, pdf) fetched automatically from filesystem',
'catalog_factory.assets.active|description' => 'Enable catalog factory for assets',
```

## Grouping of configuration

  - general
      - activate/deactivate features (whishlist, comparison, ..)
      - b2b/btoc mode
  - email
      - email addresses
  - Catalog  
      - count of product per line in product lists 
      - show left navi yes/no
      - price engine chain config
  - Checkout
      - Kind of basket footer (e.g. how totals shall be displayed)
      - Show top navi in checkout yes/no
      - columns for basket
      - offered payment options
  - User  
      - supported countries
  - Content
      - ez content IDs, etc

## Limitations

The complete parameter has to be added to the backend configuration class, it is not possible to configure only one part for the backend.

Example:

``` 
# Not possible to define this for the backend configuration:
silver_eshop.default.http_cache:
    product:
        max_age: 28800
        vary: ~

# Only the full configuration is possible:
silver_eshop.default.http_cache:
    product:
        max_age: 28800
        vary: ~
    product_list:
        max_age: 28800
        vary: ~
    product_type:
        max_age: 28800
        vary: ~
    product_type_children:
        max_age: 28800
        vary: user-hash
    price_block:
        max_age: 0
    header_login:
        max_age: 36000
        vary: cookie
    basket_preview:
        max_age: 36000
        vary: cookie
    stored_basket_preview:
        max_age: 36000
        vary: cookie
    last_viewed_products:
        max_age: 36000
        vary: cookie
    service_menu:
        max_age: 36000
        vary: ~
    navigation:
        max_age: 36000
        vary: user-hash
    newsletter:
        max_age: 36000
        vary: user-hash
```

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-12-20\_19-57-51.png](attachments/23560666/23569055.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-12-20\_19-58-48.png](attachments/23560666/23569049.png) (image/png)  
