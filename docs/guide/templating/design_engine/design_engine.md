# Design engine

!!! tip

    See [Design engine](https://doc.ezplatform.com/en/2.5/guide/design_engine/) for more information
    about using the design engine in eZ Platform.

eZ Commerce comes with the configured `base_design` and `base_theme` which use the existing standard templates used with the template resolver.

## SiteAccess configuration

You can configure designs per SiteAccess or SiteAccess group:

``` yaml
siteaccess:
    list:
      - de
      - en
      - import
      - site
      - admin
    groups:
        site_group: [de, en, import, site]
        admin_group: [admin]
    default_siteaccess: en
site_group:
    design: base_design
admin_group:
    design: admin
```

## Activation

If the template resolver of eZ Commerce is disabled (standard), the design engine of eZ Platform is automatically activated:

``` yaml
siso_tools.default.template_resolver.enabled: false
```

## Template theme paths

All eZ Commerce bundles contain an `ez_design.yml` file which is used to define the `templates_theme_path` to the templates.
Without the template theme path, the templates are not recognized by the design engine:

``` yaml
design_list:
    base_design: [base_theme]
templates_theme_paths:
    base_theme:
        - '%kernel.root_dir%/../vendor/silversolutions/silver.someBundle/Resources/views'
```
