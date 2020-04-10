# Caching in the shop

!!! tip

    eZ Commerce uses the advanced caching features of eZ Platform and Symfony.

    - http-cache 
    - esi
    - h_include
    - hmvc

## Introduction

eZ Commerce is using different caches. One of the most important cache is the http-cache which is able to speed up the shop enormously. Dynamic parts of the shop such as a basket preview or prices are displayed using dynmic caching features such as ESI or Javascript.

This ensures that only small parts of a page has to be generated in realtime.

## Usage of ESI rendered blocks

|Controller|Purpose|Cache settings|
|--- |--- |--- |
|SilversolutionsEshopBundle:CustomerProfileData:showHeaderLogin|Display infos about a user logged in in the top part of the page|Purged after login/logout and delegate process|
|SilversolutionsEshopBundle:Basket:showBasketPreview|Display a short version of the basket in the top part of the page|Purged when basket changes</br>Tags: siso_basket_<basketid></br>siso_user_<userid>|
|SilversolutionsEshopBundle:PageLayout:getFooter|Footer infos shared among all pages|caching strategy "service_menue"sec.|
|SilversolutionsEshopBundle:Bestsellers:getBestsellersEsi|Besteller Box for catalog pages|caching strategy "product_list|
|SilversolutionsEshopBundle:Bestsellers:getCategoryBestsellers</br>SilversolutionsEshopBundle:EzFlow:showLastViewedProducts|Show last viewed products e.g. on landing pages|caching strategy "product_list|
|SisoSearchBundle:Search:productList|Product list in case user is logged in|no caching|
|SilversolutionsEshopBundle:ProductType:productList|Product type list page|caching strategy "product_type_children|
|SilversolutionsEshopBundle:Basket:showStoredBasketPreview|User menu: display badge with number of products in stored comparison or number of stored baskets|caching strategy "basket_preview</br>Purged when basket changes</br>Tags: siso_basket_<basketid>|
|SilversolutionsEshopBundle:Navigation:showMenu|Left menu|Tag: siso_menu|
|SilversolutionsEshopBundle:Navigation:showMenu|Main menu|Tag: siso_menu|


\*caching strategy means:  defined in the yml file, details see: [HTTP caching](HTTP-caching_23560409.html)

## Usage of cache tags

eZ Commerce is using cache tags to tag and purge content.

|Tag|Used for|Purged by|
|--- |--- |--- |
|siso_basket_<id>|basket preview in the header|Event: on basket change|
|siso_menu|for the main menu and left side menus (product catalog)||
|content-<contentid>|Textmodules|eZ - when content (textmodules) are changed|
|siso_user_<userid>|user specific data (show name of the user)|when user logs in or out|
