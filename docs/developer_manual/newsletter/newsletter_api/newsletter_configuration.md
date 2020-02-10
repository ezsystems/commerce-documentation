#  Newsletter Configuration 

See general newsletter configuration:

**newsletter.yml**

``` 
parameters:
    #******** general newsletter configuration ************#
    # you can disable the newsletter module here
    siso_newsletter.default.newsletter_active: true
    # enable if you want to support several newsletter topics
    siso_newsletter.default.support_several_newsletters: false
    # if enabled, user will be unsubscribed from all newsletters (topics) at once
    siso_newsletter.default.unsubscribe_globally: true
    # if enabled also logged in users will see the newsletter box
    siso_newsletter.default.display_newsletter_box_for_logged_in_users: true
```
