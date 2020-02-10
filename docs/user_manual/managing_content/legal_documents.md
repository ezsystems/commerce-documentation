#  Legal Documents 

In an Online Shop you need a number of legal documents and forms. These can be managed in the folder "Service".

![](attachments/23560989/23570911.png)

The legal documents should be linked in the Footer

![](attachments/23560989/23570898.png)

The legal documents are used at various places in the shop. The T\&C is shown as an own page or as a popup during the checkout process

![](attachments/23560989/23563167.png)

and in the register form.

![](attachments/23560989/23570894.png)

The legal texts are linked in text modules that are used on various locations, i.e. the contact form.

![](attachments/23560989/23570895.png)

The Cancellation Policy text is added to the confirmation email from the shop.

### Edit

Click on "Edit" if you want to make changes. Select the language you want to edit from the drop-down list left to the "Edit" button.

### Add identifier to "Terms & Conditions"

Add an identifier to the article. With this identifier it is possible to fetch the article and render the content using a controller.

![](attachments/23560989/23563168.png)

<table>
<thead>
<tr class="header">
<th>Controller</th>
<th>Embed</th>
</tr>
</thead>
<tbody>
<tr>
<td>
<p><img src="attachments/23560989/23563170.png" class="confluence-embedded-image" /></p>
</td>
<td>
<p><img src="attachments/23560989/23563171.png" class="confluence-embedded-image" /></p>
</td>
</tr>
<tr>
<td>The article class in eZ was extended with an attribute 'identifier'. Now we can fetch the article by the given identifier and renders the content. This textmodule can be exported!</td>
<td>The issue is that such a textmodules can not be e.g. imported to another DB, because they reference to eZ content with a specific node id, that might not exist or be different in the new DB.</td>
</tr>
</tbody>
</table>

### Add new translations

[Description how content is translated in the backend](Content-translation_23560685.html)

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2016-08-04 um 11.57.02.png](attachments/23560989/23563661.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-8-4 12:3:0.png](attachments/23560989/23563662.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-8-4 12:7:1.png](attachments/23560989/23563663.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-8-4 12:11:56.png](attachments/23560989/23563713.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-8-4 12:15:22.png](attachments/23560989/23563714.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-8-4 12:18:13.png](attachments/23560989/23563715.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-8-4 15:2:52.png](attachments/23560989/23561550.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-8-4 15:12:5.png](attachments/23560989/23561548.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2016-8-4 15:19:27.png](attachments/23560989/23561552.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2017-6-6\_15-27-27.png](attachments/23560989/23563479.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2017-06-06 um 15.55.21.png](attachments/23560989/23563480.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2017-06-06 um 15.57.12.png](attachments/23560989/23563467.png) (image/png)  
![](images/icons/bullet_blue.gif) [Service.png](attachments/23560989/23570912.png) (image/png)  
![](images/icons/bullet_blue.gif) [Service.png](attachments/23560989/23570911.png) (image/png)  
![](images/icons/bullet_blue.gif) [Footer Block Service.png](attachments/23560989/23570898.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-31\_14-42-53.png](attachments/23560989/23570892.png) (image/png)  
![](images/icons/bullet_blue.gif) [Register.png](attachments/23560989/23570894.png) (image/png)  
![](images/icons/bullet_blue.gif) [contact.png](attachments/23560989/23570895.png) (image/png)  
![](images/icons/bullet_blue.gif) [checkout 5.png](attachments/23560989/23563167.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-31\_15-3-20.png](attachments/23560989/23563170.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-31\_15-4-19.png](attachments/23560989/23563171.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-7-31\_15-5-43.png](attachments/23560989/23563168.png) (image/png)  
