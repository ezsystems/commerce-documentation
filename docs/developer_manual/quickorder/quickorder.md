#  Quickorder 

## Introduction

Quickorder is an order form, that provides a fast and convenient tool for 'power'-users, which speeds up the checkout- / order-process by entering SKUs sequentially and add them altogether into the basket.

It supports autosuggestions, as well as a CSV upload.

![](attachments/23560434/23563898.png)

Analog to the [basket](Basket_23560477.html) there is a possibility to add additional data to the quickorder.

By default you can enable additional text in the quickorder:

**basket.yml**

``` 
#enable/disable additional text line in basket per basket line
ses_basket.default.additional_text_for_basket_line: false
```

## Important terms used in this document

ERP

**ERP** (Enterprise resource planning) is business [management](http://en.wikipedia.org/wiki/Management "Management") software—typically a suite of integrated applications—that a company can use to collect, store, manage and interpret data from many business activities, including:

  - [Product planning](http://en.wikipedia.org/wiki/Product_planning "Product planning"), cost
  - [Manufacturing](http://en.wikipedia.org/wiki/Manufacturing "Manufacturing") or service delivery
  - [Marketing](http://en.wikipedia.org/wiki/Marketing "Marketing") and sales
  - [Inventory management](http://en.wikipedia.org/wiki/Inventory_management "Inventory management")
  - Shipping and payment

*Source*: Wikipedia

Known ERP software packages:

  - >Microsoft Dynamics NAV (former navision)
  - Microsoft Dynamics AX  
    
  - SAP

CMS

**CMS**  (content management system) is a software for the collective creation, editing and organization of content   mostly in web pages, but also in other forms of media. This can consist  text and multimedia documents. An author with access rights can such a system, in many cases with little programming or HTML knowledge use, since the majority of systems have a graphical user interface.

*Source*: Wikipedia  

## Before you start 

Please keep in mind that Quickorder is really connected with a lot of different modules in our shop. Be sure to check these out:

  - [Catalog Element](http://confluence.ng.silverproducts.de/)
  - [ERP](http://confluence.ng.silverproducts.de/)
  - [Basket](Basket_23560477.html)

## Basic Features

### Add to basket

Users can type SKUs and quantity and either directly add products to the basket by clicking on the 'Add to basket' button, or store items in the quickorder by clicking on the button 'Update quickorder'.

### Update quickorder

If users store items in the quickorder, they will see the real prices and availability.

![](attachments/23560434/23563900.png)

If the ERP system is offline, users will see the list prices and an error message will be displayed. Also there is no information about the availability.

![](attachments/23560434/23563899.png)

Users can remove the items from the quickorder by checking the checkbox 'Delete' or entering 0 into the item quantity and then clicking on the button 'Update quickorder'.

#### How long the items will be stored?

  - if a user is *anonymous*: as long as the session exists and the user did not click on 'Add to basket button', or removed items from quickorder
  - if a user is *logged in*: as long as the user did not click on 'Add to basket' button, or removed items from quickorder

### Add more lines

If users needs more lines in his quickorder, they can add new ones by

  - either clicking on the plus icon - will add one line
  - or choosing from the drop down list and clicking on the 'Add more lines' button - will add chosen amount of lines

![](attachments/23560434/23563918.png)

## Advanced features

There are some features, such as autosuggestion or CSV upload, that makes it even easier for the user to use the quickorder form.

### Autosuggestions

*Solr* is used to look-up a list of matching results.

**Autosuggestion results returned from Solr**

![](attachments/23560434/23563919.png)

If you want to disable the autosuggest in quickorder you can use this setting:

``` 
siso_search.default.search.auto_suggest_limit: 0
```

#### Variant handling

Quickorder also supports variants. For this is the autosuggestion feature used. Users can type the product name or SKU and/or a variant code (or parts of it) into the SKU field.

*Solr* will be used to return the list of correct SKUs and variant codes.

##### Configuration

Regarding the variants, users can use different delimiters between SKU and the variant code when typing it into the SKU field.These delimiters are configurable.

``` 
parameters:
    siso_quickorder.default.autosuggest_delimiters: [' ', '/', '-', '::']
```

##### Searching for a variant

![](attachments/23560434/23563904.png)

##### Returned results

The autosuggestion results contain the SKU and the variant code separated by a configurable delimiter.

![](attachments/23560434/23563903.png)

Important

The configured delimiter MUST NOT be a part of the existing SKUs or variant codes\!

This delimiter is used to separate the variant code from the SKU when adding items into the quickorder or to the basket.

``` 
parameters:
     #this delimiter can not be a part of sku or variant code!!!
     siso_quickorder.default.sku_variant_code_delimiter: '::'
```

### Line preview

If users leave the SKU field (for example by choosing of one autosuggestion result) and set the cursor position to another field (e.g. quantity field or next line), a background search is automatically started, that will display the product name (and variant code) and the unit price. This visible feedback is a confirmation for users, which indicates that the entered SKU was correct.

![](attachments/23560434/23563905.png)

If the SKU was not correct, users will see an error message

![](attachments/23560434/23563907.png)

In order to see the real prices and the availability, users must click on the 'Update quickorder' button.

![](attachments/23560434/23563906.png)

### CSV upload

Another possibility how to add items to the quickorder is to use a CSV file. Users can either

  - upload one .csv file
  - enter a .csv file's content into a textarea

The format of the .csv file is flexible to take 1, 2 or 3 columns:

  - **SKU** - required column that has to be provided first (if only 1 column is provided there should be no delimiter at the end of the line)
  - **variant code** - optional second column. It can be omitted for the shops that do not use variants
  - **quantity** - optional last column which specifies quantity for given SKU. It is always the last column (2nd or 3rd)

Optionally, if custom text is enabled, user has the possibility to also upload custom text via csv upload. In this case the custom text shall be added as 4-th column.

``` 
 
```

    #enable/disable additional text line in basket per basket line
    ses_basket.default.additional_text_for_basket_line: false

Examples: 

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Only SKU</th>
<th>SKU and quantity</th>
<th>SKU, <strong>variant code</strong> and quantity</th>
</tr>
</thead>
<tbody>
<tr>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>SE0100
12201300
SE0101
SE0105
SE0302
13201380</code></pre>
</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>SE0100,2
12201300,3
SE0101,1
SE0105,5
SE0302,1
13201380,4</code></pre>
</td>
<td><div class="content-wrapper">
<pre class="" data-syntaxhighlighter-params="brush: java; gutter: false; theme: DJango" data-theme="DJango"><code>SE0100,,2
12201300,,3
SE0101,VAR-BLK,1
SE0105,,5
SE0302,,1
13201380,,4</code></pre>
</td>
</tr>
</tbody>
</table>

The values can by separated by a configurable delimiter:

#### Configuration

``` 
parameters:
    siso_quickorder.default.csv_delimiters: [';', ',']
```

#### Upload CSV file

Users can upload only one .csv file at once. They can either add the file by drag and drop to the upload area or use the search button.

![](attachments/23560434/23563909.png)

#### Entering CSV file content into a textarea

User can also enter the content of the .csv file directly into a textarea and click on the 'Fill quickorder' button.

![](attachments/23560434/23563873.png)

#### Result of an uploaded / entered .csv file

It doesn't matter which method was used. In both cases the quickorder will be filled with given data and appropriate messages will be displayed.

![](attachments/23560434/23563871.png)

## FAQ

Can i also search for a name of a product or variant?

Yes this is possible. An example:

The name of the product is "Entry Level Series" and some variants are named "MC-8":

![](attachments/23560431/23563854.jpg)

Which MIME types are supported for the csv upload?

Following MIME types are supported:

    'text/csv',
    'text/plain',
    'application/csv',
    'text/comma-separated-values',
    'application/excel',
    'application/vnd.ms-excel',
    'application/vnd.msexcel',
    'text/anytext',
    'application/octet-stream',
    'application/txt', 

## Templating

### Templates list:

| Path                                                                                 | Description                                                                                                                      |
| ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| Siso/Bundle/QuickOrderBundle/Resources/views/QuickOrder/quick\_order.html.twig       | Entry page for quickorder                                                                                                        |
| Siso/Bundle/QuickOrderBundle/Resources/views/QuickOrder/quick\_order\_form.html.twig | Renders the content of quickorder page                                                                                           |
| Siso/Bundle/QuickOrderBundle/Resources/views/QuickOrder/quick\_order\_line.html.twig | Renders the content of one quickorder line. This template is used to replace the quickorder line after the line preview feature. |

### Related routes:

``` 
siso_quick_order:
 pattern: /quickorder
    defaults: { _controller: SisoQuickOrderBundle:QuickOrder:quickOrder }
```

``` 

```

## Cookbook

![](http://ezcommerce-demo.ezplatform.com/bundles/silversolutionseshop/img/logo.png)

## API

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

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 08.47.57.png](attachments/23560434/23563898.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 08.50.23.png](attachments/23560434/23563900.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.05.55.png](attachments/23560434/23563899.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.06.07.png](attachments/23560434/23563918.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.06.15.png](attachments/23560434/23563919.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.06.44.png](attachments/23560434/23563904.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.07.01.png](attachments/23560434/23563903.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.07.08.png](attachments/23560434/23563905.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.07.16.png](attachments/23560434/23563907.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.07.23.png](attachments/23560434/23563906.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.07.30.png](attachments/23560434/23563909.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.07.37.png](attachments/23560434/23563873.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2015-09-18 um 09.07.48.png](attachments/23560434/23563871.png) (image/png)  
