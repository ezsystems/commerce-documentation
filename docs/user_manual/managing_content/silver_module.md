#  Silver module 

## Introduction

"silver.module" is a special solution provided by eZ Commerce, which allows backend users to load specific controllers (called "modules") by using eZ content itself.  

This means that when the user adds a page in the backend of type "silver.module", then this controller (provided in "Controller" field) will handle the request.

For example this allows the shop to dynamically call contact form (screenshot below).

![](attachments/23560987/23570902.png)

## Configuration

 "silver.module" type has the following attributes:

<table>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Attribute name
</th>
<th><div class="tablesorter-header-inner">
Description
</th>
<th><div class="tablesorter-header-inner">
Example value
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Name</td>
<td>The name of the silver.module. It is shown on the front end and is translatable.</td>
<td>My test module (results in an URL like: /Kontakt)</td>
</tr>
<tr>
<td>Controller</td>
<td><p>The controller, which should be loaded when clicking on that module. Please define the controller including the fully qualified namespace and the controller method separated by two colons (<code>::</code>).</p>
<p>The silver.module controller handler, which loads the target controller automatically validates, if the controller class and action method is available and throws an exception otherwise.</p></td>
<td><code>\Silversolutions\Bundle\EshopBundle\Controller\FormsController::formsAction</code></td>
</tr>
<tr>
<td>Parameters</td>
<td><p>Optional parameters (list of hashes) which is directed to the target controller. Keep in mind, that only string key-value pairs are possible.</p>
<p>The key value pairs are separated by a ";"</p></td>
<td>
<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><div class="tablesorter-header-inner">
Parameter key
</th>
<th><div class="tablesorter-header-inner">
Parameter value
</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>formTypeResolver</code></td>
<td><code>contact</code></td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2014-08-06 um 15.12.23.png](attachments/23560987/23563542.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-31\_14-18-36.png](attachments/23560987/23570902.png) (image/png)  
