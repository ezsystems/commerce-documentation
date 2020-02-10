#  Caching in the shop 

eZ Commerce uses the advanced caching features of eZ Platform and Symfony.

  - http-cache 
  - esi
  - h\_include
  - hmvc

## Introduction

eZ Commerce is using different caches. One of the most important cache is the http-cache which is able to speed up the shop enormously. Dynamic parts of the shop such as a basket preview or prices are displayed using dynmic caching features such as ESI or Javascript.

This ensures that only small parts of a page has to be generated in realtime.

## Usage of ESI rendered blocks

<table>
<thead>
<tr class="header">
<th>Controller</th>
<th>Purpose</th>
<th>Cache settings</th>
</tr>
</thead>
<tbody>
<tr>
<td>SilversolutionsEshopBundle:CustomerProfileData:showHeaderLogin</td>
<td>Display infos about a user logged in in the top part of the page</td>
<td>Purged after login/logout and delegate process</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Basket:showBasketPreview</td>
<td>Display a short version of the basket in the top part of the page</td>
<td><p>Purged when basket changes</p>
<p>Tags: siso_basket_&lt;basketid&gt;<br />
siso_user_&lt;userid&gt;</p></td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:PageLayout:getFooter</td>
<td>Footer infos shared among all pages</td>
<td>caching strategy "service_menue"sec.</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Bestsellers:getBestsellersEsi</td>
<td>Besteller Box for catalog pages</td>
<td>caching strategy "product_list</td>
</tr>
<tr>
<td><p>SilversolutionsEshopBundle:Bestsellers:getCategoryBestsellers</p>
<p>SilversolutionsEshopBundle:EzFlow:showLastViewedProducts</p></td>
<td>Show last viewed products e.g. on landing pages</td>
<td>caching strategy "product_list</td>
</tr>
<tr>
<td>SisoSearchBundle:Search:productList</td>
<td>Product list in case user is logged in</td>
<td>no caching</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:ProductType:productList</td>
<td>Product type list page</td>
<td>caching strategy "product_type_children</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Basket:showStoredBasketPreview</td>
<td>User menu: display badge with number of products in stored comparison or number of stored baskets</td>
<td>caching strategy "basket_preview<br />

<p>Purged when basket changes</p>
<p>Tags: siso_basket_&lt;basketid&gt;</p></td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Navigation:showMenu</td>
<td>Left menu</td>
<td>Tag: siso_menu</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Navigation:showMenu</td>
<td>Main menu</td>
<td>Tag: siso_menu</td>
</tr>
</tbody>
</table>

\*caching strategy means:  defined in the yml file, details see: [HTTP caching](HTTP-caching_23560409.html)

## Usage of cache tags

eZ Commerce is using cache tags to tag and purge content.

<table style="width:99%;">
<colgroup>
<col style="width: 35%" />
<col style="width: 35%" />
<col style="width: 28%" />
</colgroup>
<thead>
<tr class="header">
<th>Tag</th>
<th>Used for</th>
<th>Purged by</th>
</tr>
</thead>
<tbody>
<tr>
<td>siso_basket_&lt;id&gt;</td>
<td>basket preview in the header</td>
<td>Event: on basket change</td>
</tr>
<tr>
<td>siso_menu</td>
<td>for the main menu and left side menus (product catalog)</td>
<td><br />
</td>
</tr>
<tr>
<td><p>content-&lt;contentid&gt;</p></td>
<td>Textmodules</td>
<td>eZ - when content (textmodules) are changed</td>
</tr>
<tr>
<td>siso_user_&lt;userid&gt;</td>
<td>user specific data (show name of the user)</td>
<td>when user logs in or out</td>
</tr>
</tbody>
</table>

## FAQ

How to solve the problem that the basket preview is not up to date and the box showing the user logged in as well?

This might be caused by a invalid configuration.

Please check if the setting in ezplatform.yml for purging the cache is set correctly:

``` 
ezpublish:
    http_cache:
        purge_type: local
```

  - local means the proxy of Symfony is used
  - if varnish or a CDN is used set purge\_type to http

eZ Commerce uses the advanced caching features of eZ Platform and Symfony.

  - http-cache 
  - esi
  - h\_include
  - hmvc

## Introduction

eZ Commerce is using different caches. One of the most important cache is the http-cache which is able to speed up the shop enormously. Dynamic parts of the shop such as a basket preview or prices are displayed using dynmic caching features such as ESI or Javascript.

This ensures that only small parts of a page has to be generated in realtime.

## Usage of ESI rendered blocks

<table>
<thead>
<tr class="header">
<th>Controller</th>
<th>Purpose</th>
<th>Caching settings</th>
</tr>
</thead>
<tbody>
<tr>
<td>SilversolutionsEshopBundle:CustomerProfileData:showHeaderLogin</td>
<td>Display infos about a user logged in in the top part of the page</td>
<td>Purged after login/logout and delegate process</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Basket:showBasketPreview</td>
<td>Display a short version of the basket in the top part of the page</td>
<td><p>Purged when basket changes</p>
<p>Tags:</p>
<pre><code>siso_basket</code></pre></td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:PageLayout:getFooter</td>
<td>Footer infos shared among all pages</td>
<td><br />
</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Bestsellers:getBestsellersEsi</td>
<td>Besteller Box for catalog pages</td>
<td><br />
</td>
</tr>
<tr>
<td><p>SilversolutionsEshopBundle:Bestsellers:getCategoryBestsellers</p>
<p>SilversolutionsEshopBundle:EzFlow:showLastViewedProducts</p></td>
<td>Show lats viewed products e.g. on landing pages</td>
<td><br />
</td>
</tr>
<tr>
<td>SisoSearchBundle:Search:productList</td>
<td>Product list in case user is logged in</td>
<td><br />
</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:ProductType:productList</td>
<td>Product type list page</td>
<td><br />
</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Basket:showStoredBasketPreview</td>
<td>User menu: display badge with number of products in stored comparison or number of stored baskets</td>
<td><br />
</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Navigation:showMenu</td>
<td>Left menu</td>
<td>Tag: siso_menu</td>
</tr>
<tr>
<td>SilversolutionsEshopBundle:Navigation:showMenu</td>
<td>Main menu</td>
<td>Tag: siso_menu</td>
</tr>
</tbody>
</table>

## Usage of cache tags

eZ Commerce is using cache tags to tag and purge content.

<table style="width:99%;">
<colgroup>
<col style="width: 35%" />
<col style="width: 35%" />
<col style="width: 28%" />
</colgroup>
<thead>
<tr class="header">
<th>Tag</th>
<th>Used for</th>
<th>Purged by</th>
</tr>
</thead>
<tbody>
<tr>
<td>siso_basket_&lt;id&gt;</td>
<td>basket preview in the header</td>
<td><br />
</td>
</tr>
<tr>
<td>siso_menu</td>
<td>for the main menu and left side menus (product catalog)</td>
<td><br />
</td>
</tr>
<tr>
<td>textmodule</td>
<td><br />
</td>
<td><br />
</td>
</tr>
<tr>
<td>header login</td>
<td><br />
</td>
<td><br />
</td>
</tr>
</tbody>
</table>

## FAQ

How to solve the problem that the basket preview is not up to date and the box showing the user logged in as well?

This might be caused by a invalid configuration.

Please check if the setting in ezplatform.yml for purging the cache is set correctly:

``` 
ezpublish:
    http_cache:
        purge_type: local
```

  - local means the proxy of Symfony is used
  - if varnish or a CDN is used set purge\_type to http
