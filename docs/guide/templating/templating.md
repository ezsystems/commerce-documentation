# Templating

eZ Commerce offers additional Twig functions and filters you can use in templates.

## Template resolver

eZ Commerce comes with a [template resolver](template_resolver.md) system which enables overriding templates on a SiteAccess and design base. 

## Design engine

Instead of the template resolver you can use the [design engine](https://doc.ezplatform.com/en/2.5/guide/design_engine/).
See [Integration of the design engine](design_engine/design_engine.md) for more information.

## Shop templates

Built-in templates for the shop are stored in multiple bundles

``` 
vendor/silversolutions/silver.customercenter/src/Siso/Bundle/CustomerCenterBundle/Resources/views
vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/views
vendor/silversolutions/silver.orderhistory/src/Siso/Bundle/OrderHistoryBundle/Resources/views
```
