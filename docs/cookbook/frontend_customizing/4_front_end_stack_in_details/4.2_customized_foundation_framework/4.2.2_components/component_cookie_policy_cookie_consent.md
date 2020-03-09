# Component - Cookie policy (cookie consent)

Cookie policy is a component based on cookie consent jQuery plugin: http://privacypolicies.com/cookie-consent/. 

It allows to show a notification message about EU cookie policy law: http://www.cookielaw.org/the-cookie-law/.

## Sass

**File location**

``` 
scss/storm/_components.cookiepolicy.scss
```

## HTML

Required piece of HTML code you need to place somewhere in the template. We recommend placing it somewhere in the footer area so users are not distracted to much with it.

``` 
<span class="cookie_link" data-policy-link="link/to/policy/pafe">
```

## Twig

We place it in the footer area directly in pagelayout.html.twig file.

``` 
{% if ses.profile.sesUser.isAnonymous %}<span class="right cookie_link" data-policy-link="path/to/text/module">{% endif %}
```

We don't show cookie policy to logged in users no matter what. Our login functionality is based on cookies so users must accept it.

## JavaScript

**Files location**

``` 
js/cookiepolicy/storm.cookiepolicy.actions.js
js/cookiepolicy/storm.cookiepolicy.js
```

### Configuration

**Default settings**

``` 
cookie_name: 'cookie_policy_level',
// default to 365 days in the future
cookie_expires: new Date((new Date()).getTime() + 365 * 24 * 60 * 60 * 1000), 
autorun: true,
// popup element
popup_container: 'body',
// an element that when clicked will let the user edit their settings. if the element is empty, this plugin will add some call to action text
edit_settings_element: '#edit-cp-settings',
// callback for when the level is changed *by the user*
on_change: function () {},
// url of a page explaining how cookies are to be used on the website
cookie_policy_url: $('.cookie_link').data('policy-link'),
// settings levels, starting with basic (legal for all sites) and incrementally getting more cookies set
// when the run() method is called, callback functions will be invoked from level index 0 through to the current level
levels: [{}],
// strategy defines the way how user can interact
// simple - just an information with OK button
// advanced - close icon + button to show a modal window with different levels
strategy: 'simple'
```

In order to change configuration please open js/cookiepolicy/storm.cookiepolicy.actions.js override any of properties above. Here's an example:

``` 
var settings = {
  popup_container: $('.js-main-section')
};
 
$.cookiepolicy(settings);
```
