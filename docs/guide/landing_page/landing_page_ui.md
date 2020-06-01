# Landing Page UI

The built-in Landing Page blocks use templates which can be overridden in a project.

|Block|Template|Subtemplates|
|--- |--- |--- |
|Last viewed items|`SisoEzStudioBundle:blocks:product_last_viewed_slider.html.twig`|Uses the `SilversolutionsEshopBundle:EzFlow:showLastViewedProducts` subcontroller and `SilversolutionsEshopBundle:Catalog:last_viewed_slider.html.twig` template|
|Bestsellers|`SisoEzStudioBundle:blocks:bestseller.html.twig`|Uses the `SilversolutionsEshopBundle:Bestsellers:getBestsellers` subcontroller and the `SilversolutionsEshopBundle:Bestsellers:bestsellers_box.html.twig` template|
|Product slider|`SisoEzStudioBundle:blocks:product_slider.html.twig`|Uses the `SilversolutionsEshopBundle:EzFlow:getSkuListByString` subcontroller and the `SisoEzStudioBundle:blocks:product_slider_tabs.html.twig` template|
