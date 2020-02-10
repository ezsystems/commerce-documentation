#  One-page forms - Templates 

### Templates list:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Path</p>
<pre><code>Silversolutions/Bundle/EshopBundle/Resources/views/Forms/*</code></pre></th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>register_business.html.twig</code></pre></td>
<td>Renders the business registration form</td>
</tr>
<tr>
<td><pre><code>register_private.html.twig</code></pre></td>
<td>Renders the private registration form</td>
</tr>
<tr>
<td><pre><code>activate_business.html.twig</code></pre></td>
<td>Renders the account activation form for existing customers</td>
</tr>
<tr>
<td><pre><code>my_account.html.twig</code></pre></td>
<td>Renders the update my account data form</td>
</tr>
<tr>
<td><pre><code>buyer.html.twig</code></pre></td>
<td>Renders the update the buyer address form</td>
</tr>
<tr>
<td><pre><code>invoice.html.twig</code></pre></td>
<td>Renders update the invoice address form</td>
</tr>
<tr>
<td><pre><code>password_change.html.twig</code></pre></td>
<td>Renders the change password form</td>
</tr>
<tr>
<td><pre><code>password_reminder.html.twig</code></pre></td>
<td>Renders the forgot password form</td>
</tr>
<tr>
<td><pre><code>cancellation.html.twig</code></pre></td>
<td>Renders the online cancellation form (RMA)</td>
</tr>
<tr>
<td><pre><code>contact.html.twig</code></pre></td>
<td>Renders the contact us form</td>
</tr>
<tr>
<td>Subtemplates</td>
<td> </td>
</tr>
<tr>
<td><pre><code>party.html.twig</code></pre></td>
<td>Renders the party form attributes</td>
</tr>
<tr>
<td><pre><code>custom_form_label.html.twig</code></pre></td>
<td>Renders custom template for the form label</td>
</tr>
</tbody>
</table>

### Related custom Twig modifiers/functions/etc/:

``` 
# define a route to your form, pass the formTypeResolver as a parameter
<a href="{{ path('silversolutions_service', {'formTypeResolver': 'registration_private'}) }}">{{ 'msg.register_here'|st_translate }}</a>
```

### Related routes:

**Alternative routes for the FormsController::formsAction**

``` 
silversolutions_forms:
    path:  /register/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }

silversolutions_profile:
    path:  /profile/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }

silversolutions_password_reminder:
    path:  /password/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }

silversolutions_service:
    path:  /service/{formTypeResolver}
    defaults: { _controller: SilversolutionsEshopBundle:Forms:forms }
```
