#  Component - Social share 

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

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>data-info-link</td>
<td>If you use <code>box</code> as you <code>perma-type</code> you can also specify a link to the <em>legal terms</em> or other section on you page that describes cookies, social services and any other important parts related to this topic. <br />
Default: <em>''</em> - empty string</td>
</tr>
<tr>
<td>data-perma-type</td>
<td>Social Share Plugins allows you for choosing permanent options in two different variants:
<ul>
<li><em>checkbox</em> - shows checkbox right next to the widget for each social service after you enable it</li>
<li><em>box</em> - shows all services together in a dropdown layer (settings box)</li>
</ul></td>
</tr>
<tr>
<td>data-facebook-status</td>
<td>Enable / disable Facebook Like widget. It allows for: <em><strong>on</strong></em> or <em><strong>off</strong></em>, if empty or doesn't exist it will be on (default) <br />
Default: <em>on</em></td>
</tr>
<tr>
<td>data-facebook-share</td>
<td><p>Gives the possibility to show the Share button right next to the like button. Possible values: true or false</p>
<p>Default: false</p></td>
</tr>
<tr>
<td>data-gplus-status</td>
<td>Enable / disable Tweeter Tweet widget. It allows for: <em><strong>on</strong></em> or <em><strong>off</strong></em>, if empty or doesn't exist it will be on (default) <br />
Default: <em>on</em></td>
</tr>
<tr>
<td>data-twitter-status</td>
<td><p>Enable / disable Google +1 widget. It allows for:<em><strong>on</strong></em> or <em><strong>off</strong></em>, if empty or doesn't exist it will be on (default) <br />
Default: <em>on</em></p>

<p>Please keep in mind that Google +1 is not supported for Internet Explorer 7 or below.</p>

</td>
</tr>
<tr>
<td>data-pinterest-status</td>
<td><p>Enable / disable Pinterest Pin It widget. It allows for:<em><strong>on</strong></em> or <em><strong>off</strong></em>, if empty or doesn't exist it will be on (default) <br />
Default: <em>on</em></p>
<p> </p>

<p>Please keep in mind that Pinterest Pin It is not supported in Internet Explorer 9 or below.</p>

</td>
</tr>
</tbody>
</table>

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
