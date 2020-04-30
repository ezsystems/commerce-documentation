# Templating

## Introduction

eZ Commerce uses Twig templates for the frontend and backend features as well. Twig is a very powerful template language which allow a very flexible way to change the frontend of the shop and platform. 

All Template can be extended or overridden in projects if required. 

For a general documentation about the template language we recommend the documentation from Symfony:

http://twig.sensiolabs.org/

## Provided template functions

### Functions

|Operator|Features|Docu|
|--- |--- |--- |
|st_tag|Injects data tags for google tag manager or BDD tests|st_tag - selector tagging for Behat and Google Tag manager|
|ses_render_field|renders a product field|Twig extension|
|ses_navigation|Returns navigation node tree|Twig extension|
|ses_product|Returns a product node|Twig extension|
|ses_variant_product_by_sku|Fetches a VariantProductNode for given sku|Twig extension|
|ses_assets_main_image|Returns the main image of catalogElement or null|Twig extension|
|ses_assets_image_list|Returns the list of images for a catalogElement or an empty array|Twig extension|
|ses_variants_sort|Returns all product variants in a given order|Twig extension|
|ses_render_price|Renders a PriceField|Twig extension|
|ses_render_stock|Renders a StockField|Twig extension|
|ses_render_specification_matrix|Renders a specification MatrixField|Twig extension|

### Filters

|Name|Description||
|--- |--- |--- |
|date_format|Formats a DATE value|Twig extension|
|price_format|Formats a price value</br>default parameters in silver.eshop.yml</br>silver_eshop.default.price_format_currency: EUR</br>silver_eshop.default.price_format_locale: en|Twig extension|
|youtube_video_id|Returns a video ID from a Youtube URL|Twig extension|
|shipping|Returns the shipping costs from the basket or null|Twig extension|
|code_label|Returns the translated code label. This can be e.g. country code or different code like mrs.</br>if the context is 'country' first Symfony Intl Bundle is used to resolve the country code to country name otherwise just the translation for the code is returned|Twig extension|

## Template resolver - a flexible template solution

eZ Commerce comes with a very flexible template resolver system which allows to override templates on a siteaccess and design base. 

Please check the documentation:

[Template resolver](template_resolver.md)

## eZ Design Engine

eZ Commerce is available and can be used instead of the Template resolver.

Please check the following documentations for detailed information:

- [Integration of eZ Design Engine](ezdesign_engine/ez_design_engine.md)
- [eZ Design Engine documentation](https://doc.ezplatform.com/en/latest/guide/design_engine/)
