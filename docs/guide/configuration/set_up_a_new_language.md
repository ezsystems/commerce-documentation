# Set up a new language

!!! note

    If a new SiteAccess is introduced it is important to add a new Policy for the anonymous user Role.

    Allow login for the new SiteAccess:

    ![](../img/configuration.png)

## Configuration

- `ezpublish.siteaccess.list` - general configuration for new SiteAccess
- `ezpublish.siteaccess.groups.ezdemo_site_clean_group` - assign the new SiteAccess to general group `ezdemo_site_clean_group`
- `ezpublish.siteaccess.match.Compound\LogicalAnd` - define the domain and language for this new SiteAccess, e.g.:

``` yaml
website_be_nl:
    matchers:
        Map\URI:
            nl: true
        Map\Host:
            %domain_website_be%: true
    match: website_be_nl
```

Next, configure session name and used languages:

``` yaml
website_be_nl:            
    session:
        name: eZSESSID
    languages:
        - dut-NL
```

- `ezpublish.siteaccess.ses_admin.languages` - the new language must be enabled for admin SiteAccess.
- `ezsettings.default.languages` - the new language must be added.
