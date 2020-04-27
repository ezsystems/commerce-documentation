# eZ Design Engine

This document is about the integration of the Design Engine in eZ Commerce.

For detailed information about the Design Engine read the [eZ Design Engine documentation](https://doc.ezplatform.com/en/latest/guide/design_engine/).

## General

eZ Commerce comes with a configured base\_design and base\_theme which is using the existing standard templates used with the template resolver.

## Configurations

### SiteAccess configuration

It's possible to configure designs per siteaccess or group

`app/config/ezplatform.yml`:

``` yaml
# Siteaccess configuration, with one siteaccess per default
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
#Frontend
site_group:
    design: base_design
#Backend
admin_group:
    design: admin
```

### Activation

If the Template resolver of eZ Commerce is deactivated (standard) the design engine of eZ Platform is automatically active:

`ToolsBundle/Resources/config/siso.tools.yml`:

``` 
siso_tools.default.template_resolver.enabled: false
```

Template theme paths

All eZ Commerce bundles contain an `ez_design.yml` which is used to define the `templates_theme_path` to the templates. It is necessary due to the old bundle structure. Without the template theme path the templates will not be recognized by the Design Engine:

`someBundle/Resources/config/ez_design.yml`

``` yaml
design_list:
    base_design: [base_theme]
templates_theme_paths:
    base_theme:
        - '%kernel.root_dir%/../vendor/silversolutions/silver.someBundle/Resources/views'
```
