# Landing Page UI

The prepared Landing Page blocks are using templates which can be overridden in a project.

|Block|Used template|Used sub templates|
|--- |--- |--- |
|Last viewed items|SisoEzStudioBundle:blocks:product_last_viewed_slider.html.twig|Uses a Subcontroller SilversolutionsEshopBundle:EzFlow:showLastViewedProducts and the template:</br>SilversolutionsEshopBundle:Catalog:last_viewed_slider.html.twig|
|Bestsellers|SisoEzStudioBundle:blocks:bestseller.html.twig|User a subcontroller SilversolutionsEshopBundle:Bestsellers:getBestsellers and the template:</br>SilversolutionsEshopBundle:Bestsellers:bestsellers_box.html.twig|
|Product slider|SisoEzStudioBundle:blocks:product_slider.html.twig|Uses a Subcontroller SilversolutionsEshopBundle:EzFlow:getSkuListByString' and the template:</br>SisoEzStudioBundle:blocks:product_slider_tabs.html.twig|
