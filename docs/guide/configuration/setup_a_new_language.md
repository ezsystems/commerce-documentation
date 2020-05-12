# Set up a new language

!!! note

    If a new SiteAccess is introduced it is important to add a new Policy for the anonymous user Role:

    Allow login for the new SiteAccess:

    ![](../img/configuration.png)

## Configuration

`app/config/ezplatform.yml`

- ezpublish.siteaccess.list - General configuration for new SiteAccess
- ezpublish.siteaccess.groups.ezdemo_site_clean_group - Set new SiteAccess to general group `ezdemo_site_clean_group`
- ezpublish.siteaccess.match.Compound\LogicalAnd - Define the domain and language for this new SiteAccess, e.g.:

``` yaml
website_be_nl:
    matchers:
        Map\URI:
            nl: true
        Map\Host:
            %domain_website_be%: true
    match: website_be_nl
```

Configuration for session name and used languages.

``` yaml
website_be_nl:            
    session:
        name: eZSESSID
    languages:
        - dut-NL
```

- ezpublish.siteaccess.ses_admin.languages - New language must be enabled for admin siteaccess.
- ezsettings.default.languages - New language must be added.
