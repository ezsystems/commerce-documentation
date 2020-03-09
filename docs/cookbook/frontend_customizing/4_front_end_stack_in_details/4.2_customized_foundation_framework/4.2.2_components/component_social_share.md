# Component - Social share

Social Share plugin supports:

- Facebook Like Button
- Tweeter Tweet
- Google +1 Button
- Pinterest Pin it Button
- LinkedIn Share Button
- Xing Share Button
- Print page option

## Javascript 

**File location**

``` 
js/socialshare/storm.socialshare.actions.js
js/socialshare/storm.socialshareprivacy.js
```

## Sass

**File location**

``` 
scss/storm/_components.socialshare.scss
```

## Twig / HTML

### Settings

It's possible to use HTML5 data-\* attributes to change settings.

|Attribute name|Description|
|--- |--- |
|data-info-link|If you use box as you perma-type you can also specify a link to the legal terms or other section on you page that describes cookies, social services and any other important parts related to this topic. </br>Default: '' - empty string|
|data-perma-type|Social Share Plugins allows you for choosing permanent options in two different variants:</br>checkbox - shows checkbox right next to the widget for each social service after you enable it</br>box - shows all services together in a dropdown layer (settings box)|
|data-facebook-status|Enable / disable Facebook Like widget. It allows for: on or off, if empty or doesn't exist it will be on (default)</br>Default: on|
|data-facebook-share|Gives the possibility to show the Share button right next to the like button. Possible values: true or false</br>Default: false|
|data-gplus-status|Enable / disable Tweeter Tweet widget. It allows for: on or off, if empty or doesn't exist it will be on (default)</br>Default: on|
|data-twitter-status|Enable / disable Google +1 widget. It allows for:on or off, if empty or doesn't exist it will be on (default)</br>Default: on</br>Please keep in mind that Google +1 is not supported for Internet Explorer 7 or below.|
|data-pinterest-status|Enable / disable Pinterest Pin It widget. It allows for:on or off, if empty or doesn't exist it will be on (default)</br>Default: on</br>Please keep in mind that Pinterest Pin It is not supported in Internet Explorer 9 or below.|

### Examples

#### Basic example

``` 
<div class="socialshareprivacy">
```

*This will enable all social share services available (Facebook, Tweeter, Google+, Pintereset)*

#### Disable some services

``` 
<div class="socialshareprivacy" data-gplus-status="off" data-pintereset-status="off"> 
```

*_This will disable Google+ and Pintereset._*

#### Enable Facebook share button

``` 
<div class="socialshareprivacy" data-facebook-share="true">
```
