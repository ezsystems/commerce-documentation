#  Customer - fields in the CMS 

# Contenttype user in the CMS

By default the eZ User class contains following fields:

<table>
<thead>
<tr class="header">
<th>Field</th>
<th>Field identifier</th>
<th>Datatype</th>
<th>Notice</th>
</tr>
</thead>
<tbody>
<tr>
<td>Name</td>
<td><code>name</code></td>
<td>Text line</td>
<td><p>This attribute can <strong>not</strong> be removed from the User class.</p>
<p>There are users (like admin, editors) who don´t have any customer_profile_data information.</p></td>
</tr>
<tr>
<td>First name</td>
<td><pre><code>first_name</code></pre></td>
<td>Text line</td>
<td><br />
</td>
</tr>
<tr>
<td>Last name</td>
<td><pre><code>last_name</code></pre></td>
<td>Text line</td>
<td><br />
</td>
</tr>
<tr>
<td>Salutation</td>
<td><code>salutation</code></td>
<td><a href="SesSelection_23560397.html">SesSelection</a></td>
<td>The User salutation is stored here</td>
</tr>
<tr>
<td>User Account</td>
<td><code>user_account</code></td>
<td>User account</td>
<td><br />
</td>
</tr>
<tr>
<td>Signature</td>
<td><code>signature</code></td>
<td>Text block</td>
<td><br />
</td>
</tr>
<tr>
<td>Image</td>
<td><code>image</code></td>
<td>Image</td>
<td><br />
</td>
</tr>
<tr>
<td>Customer number</td>
<td><code>customer_number</code></td>
<td>Text line</td>
<td><br />
</td>
</tr>
<tr>
<td>Contact Number</td>
<td><code>contact_number</code></td>
<td>Text line</td>
<td><br />
</td>
</tr>
<tr>
<td>Customer Profile Data</td>
<td><code>customer_profile_data</code></td>
<td><p>sesprofiledata</p>
<p><br />
</p></td>
<td><p>This field contains a Base64 encoded string.</p>
<p>If decoded and unserialized, this results in a <a href="Customer-profile-data-model_23560898.html">customer profile data model entity</a>.</p></td>
</tr>
<tr>
<td>Budget per order</td>
<td><pre><code>budget_order</code></pre></td>
<td>Float</td>
<td><br />
</td>
</tr>
<tr>
<td>Budget per month</td>
<td><pre><code>budget_month</code></pre></td>
<td>Float</td>
<td><br />
</td>
</tr>
</tbody>
</table>

## Access to the profile

Not every user is allowed to modify his profile data in the shop (e.g. guest). You have to add the policy "siso\_policy/forms\_profile\_edit" explicit to a role and assign it to customers. The standard already provides a role "Ecommerce registered users" which includes this policy. 

### Getting the policy in the Controller

``` 
 return $this->render(
            'SilversolutionsEshopBundle::details.html.twig',
            array(
                'grant_profile_edit' => $this->isGranted(new AuthorizationAttribute('siso_policy', 'forms_profile_edit')),
            )
        );
```

### Using the policy in the Template

``` 
{% if grant_profile_edit %}
    <a href="{{ path('silversolutions_profile', {'formTypeResolver': 'my_account'}) }}" class="button float_left">{{ 'Change My Account'|st_translate('profile') }}</a>
    <div class="float_left">&nbsp;
    <a href="{{ path('ez_legacy', {'module_uri': '/user/password'}) }}" class="button float_left">{{ 'Change password'|st_translate() }}</a>
{% endif %}
```

### Prohibit access to the form

You can also prohibit access to the form if the user calls a specific URL, where he does not have required role. This is set in the [configuration](/pages/createpage.action?spaceKey=EZC14&title=Forms+Configuration&linkCreation=true&fromPageId=23560679).

**forms.yml**

``` 
    ses_forms.configs.buyer:
        ...
        policy: siso_policy/forms_profile_edit
        ... 
```

Example:

User calls URL /profile/buyer (wants to edit the buyer address) without appropriate role.
