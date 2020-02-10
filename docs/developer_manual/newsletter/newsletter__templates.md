#  Newsletter -Templates 

### Templates list:

<table>
<thead>
<tr class="header">
<th>Path</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Newsletter/newsletter_box.html.twig</code></pre></td>
<td><p>Renders newsletter box. see <a href="#Newsletter-Templates-newsletter_box">Newsletter Box</a>.</p></td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Newsletter/newsletter_message.html.twig</code></pre></td>
<td>Renders a simple page with success/error messages after user subscribed/unsubscribed to newsletter</td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_SubscribeNewsletter.html.twig</code></pre></td>
<td>HTML confirmation email that is send to the user in the DOI process</td>
</tr>
<tr>
<td><pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Emails/ConfirmationMail_SubscribeNewsletter.txt.twig</code></pre></td>
<td>Text confirmation email that is send to the user in the DOI process</td>
</tr>
</tbody>
</table>

### Related routes:

``` 
siso_newsletter_subscribe:
    path:     /newsletter/subscribe
    defaults:
        _controller: SisoNewsletterBundle:Newsletter:subscribeNewsletter
        breadcrumb_path: siso_newsletter_subscribe
        breadcrumb_names: subscribe newsletter

siso_newsletter_unsubscribe:
    path:     /newsletter/unsubscribe
    defaults:
        _controller: SisoNewsletterBundle:Newsletter:unsubscribeNewsletter
        breadcrumb_path: siso_newsletter_unsubscribe
        breadcrumb_names: common.unsubscribe_newsletter

siso_newsletter_update:
    path:     /newsletter/update
    defaults:
        _controller: SisoNewsletterBundle:Newsletter:updateNewsletter
        breadcrumb_path: silversolutionsCustomerDetail/siso_newsletter_update
        breadcrumb_names: My profile/common.update_newsletter

siso_newsletter_doi:
    path:     /newsletter/doi
    defaults:
        _controller: SisoNewsletterBundle:Newsletter:doubleOptInNewsletter
        breadcrumb_path: silversolutionsCustomerDetail/siso_newsletter_subscribe
        breadcrumb_names: My profile/subscribe newsletter
```

### Newsletter Box

The newsletter box can be rendered as an esi block and can be rendered in the Page Builder as well. The box is cached per user.

Therefore the block [SesTextAndBanner](/pages/createpage.action?spaceKey=EZC14&title=Landing+page+tool+-+blocks&linkCreation=true&fromPageId=23560212) has been extended, so objects from type silver.module can be added to this block as well.

All paramaters from the block template are forwarded and accesible here, example:

**Silversolutions/Bundle/EshopBundle/Resources/views/block/text\_and\_banner\_50.html.twig**

``` 
{# image_alias and view will be accessible in the newsletter_box.html.twig #}
{# therefore is is possible to display different width depending on the view #}

{{ render(
  controller(
    'ez_content:viewLocation',
    {
      'locationId': item.locationId,
      'viewType': 'block_item',
      'params' : {'image_alias' : image_alias, 'view' : 50}
    })
  ) 
}} 
```

#### View 50

![](attachments/23560212/23570778.png)

#### View 100

![](attachments/23560212/23570780.png)

## How to render a newsletter form

You can use a render statement to render the newlsetter form in a template

``` 
{{ render(
    controller(
        'SisoNewsletterBundle:Newsletter:renderNewsletterBox',
        {'params' : { 'display_hr' : true }}
    )
) }}
```

## Attachments:

![](images/icons/bullet_blue.gif) [Bildschirmfoto 2016-11-14 um 14.33.06.png](attachments/23560212/23570778.png) (image/png)  
![](images/icons/bullet_blue.gif) [Bildschirmfoto 2016-11-14 um 14.33.13.png](attachments/23560212/23570780.png) (image/png)  
