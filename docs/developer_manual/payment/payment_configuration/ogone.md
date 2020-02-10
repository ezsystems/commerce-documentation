#  Ogone 

Ogone is a payment system offered by Ingenico e-Commerce Solutions: <http://payment-services.ingenico.com/gb/en/about-ingenico-payment-services>

## Important links

<table>
<thead>
<tr class="header">
<th><br />
</th>
<th><br />
</th>
</tr>
</thead>
<tbody>
<tr>
<td>Backend</td>
<td><a href="https://secure.ogone.com/Ncol/Test/backoffice/container/index?branding=OGONE" class="external-link">https://secure.ogone.com/Ncol/Test/backoffice/container/index?branding=OGONE</a></td>
</tr>
<tr>
<td><p>Error codes / messages</p></td>
<td><a href="https://payment-services.ingenico.com/int/en/ogone/support/guides/integration%20guides/possible-errors" class="external-link">https://payment-services.ingenico.com/int/en/ogone/support/guides/integration%20guides/possible-errors</a></td>
</tr>
</tbody>
</table>

## Configuration parameters

``` 
ets_payment_ogone:
    pspid: my psid
    shain: "<shain defined in ogone backend>"
    shaout: "<shaout defined in ogone backend>"
    debug: true
    utf8: true
    api:
        user: shop_user_id
        password: "<password defined for user in ogone backend>"
```

<table>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Details</th>
</tr>
</thead>
<tbody>
<tr>
<td><pre><code>pspid</code></pre></td>
<td>The psid used for log in in the ogone backend.</td>
</tr>
<tr>
<td><pre><code>shain</code></pre></td>
<td>has to be <a href="#Ogone-6)Dataandoriginverification">defined in the ogone backend/administration</a></td>
</tr>
<tr>
<td><pre><code>shaout</code></pre></td>
<td>has to be <a href="#Ogone-7)SetShaOutPasswordandnotificationURLs">defined in the ogone backend/administration</a></td>
</tr>
<tr>
<td><pre><code>api/user</code></pre></td>
<td>The user (see "<a href="#Ogone-3)Createauserfortheshop">Create a user for the shop</a>")</td>
</tr>
<tr>
<td><pre><code>api/password</code></pre></td>
<td>The password set for this user</td>
</tr>
</tbody>
</table>

This configuration is not site-access aware. For the case that multiple Ogone accounts must be configured, depending on the site-access, the token service must be redefined in order to get site-access aware parameters injected:

**Example**

``` 
         <!-- Overridden token service for Ogone -->
        <service id="payment.ogone.client.token" class="%payment.ogone.client.token.class%">
            <argument>$payment_ogone_pspid;project_domain$</argument>
            <argument>$payment_ogone_api_username;project_domain$</argument>
            <argument>$payment_ogone_api_password;project_domain$</argument>
            <argument>$payment_ogone_shain;project_domain$</argument>
            <argument>$payment_ogone_shaout;project_domain$</argument>
        </service>
```

Fortunately, only this single service relies on the account parameters. In order to configure the values effectively, the ets\_payment\_ogone configuration can't be used any longer. The configuration above would now look like:

``` 
project_domain.site_access_name.payment_ogone_pspid: my psid
project_domain.site_access_name.payment_ogone_shain: "<shain defined in ogone backend>"
project_domain.site_access_name.payment_ogone_shaout: "<shaout defined in ogone backend>"
project_domain.site_access_name.payment_ogone_api_username: shop_user_id
project_domain.site_access_name.payment_ogone_api_password: "<password defined for user in ogone backend>"
```

**PLEASE NOTE**: debug and utf8 must still be configured for the config namespace ets\_payment\_ogone and are NOT site-access aware.

## How to setup the account in Ogone

### 1\) Activate necessary 3-tiers option

It is important to activate the Option "e-Commerce 3-tiers access" (or in German: e-Commerce 3-tiers Zugriff).

Click on Configuration-\>Account-\>Your Options

![](attachments/23560639/23563440.png)

### 2\) Activate a payment such as VISA

Click on Configuration-\>Payment methods

![](attachments/23560639/23563439.png)

### 3\) Create a user for the shop

Click on Configuration-\>Users

**Important**: Please activate the option "API user".

![](attachments/23560639/23563438.png)

### 4\) Configure Global transaction parameters

If the shop shall just authorize the transaction and the ERP will finalize the transaction then the option "Authorization" has to be set. 

![](attachments/23560639/23563443.png)

### 5\) Global security parameters

Please choose SHA-1 as has algorithm and UTF-8 as encoding standard. 

![](attachments/23560639/23563445.png)

### 6\) Data and origin verification

Please type in a secure SHA-in pass phrase and add the SHA-in pass phrase in the settings of the shop 

![](attachments/23560639/23563456.png)

### 7\) Set ShaOut Password and notification URLs

Go to link Configuration -\>Technical information -\>Transaction Feedback

![](attachments/23560639/23563786.png)

![](attachments/23560639/23563420.png)

![](attachments/23560639/23563836.jpg)

## Attachments:

![](images/icons/bullet_blue.gif) [TEST\_Ogone\_payment\_server\_\_administration.png](attachments/23560639/23563440.png) (image/png)  
![](images/icons/bullet_blue.gif) [Ogone-Payment-Options1.png](attachments/23560639/23563439.png) (image/png)  
![](images/icons/bullet_blue.gif) [ogone-user.png](attachments/23560639/23563438.png) (image/png)  
![](images/icons/bullet_blue.gif) [Ogone-transaction-parameters.png](attachments/23560639/23563443.png) (image/png)  
![](images/icons/bullet_blue.gif) [ogone-global-security.png](attachments/23560639/23563445.png) (image/png)  
![](images/icons/bullet_blue.gif) [Ogone-data-verification.png](attachments/23560639/23563456.png) (image/png)  
![](images/icons/bullet_blue.gif) [ogone shaout.jpg](attachments/23560639/23563836.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [ogone\_notify.png](attachments/23560639/23563420.png) (image/png)  
![](images/icons/bullet_blue.gif) [ogone\_redirection.png](attachments/23560639/23563786.png) (image/png)  
