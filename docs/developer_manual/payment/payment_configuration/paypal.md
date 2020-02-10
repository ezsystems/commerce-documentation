#  PayPal 

## Express Checkout

## Installation and configuration

For Express Checkout payments a third party library (JMSPaymentPaypalBundle) is necessary. For installation and setup please look at: <http://jmspaymentpaypalbundle.readthedocs.io/en/stable/setup.html>

``` 
php composer.phar require jms/payment-paypal-bundle
php composer.phar update -- jms/payment-paypal-bundle
```

Please see [How to get the API credentials](#PayPal-HowtogettheAPIcredentials) to learn were the values for the JMSPaymentPaypalBundle configuration can be found in the PayPal merchant's administration.

Additionally, the SisoPaypalPaymentBundle must be activated in the Kernel and the routes must be included:

**app/AppKernel.php**

``` 
public function registerBundles()
{
    $bundles = array(
        // ...
        new Siso\Bundle\PaypalPaymentBundle\SisoPaypalPaymentBundle(),
    );
}
```

**app/config/routing.yml**

``` 
# ...
_siso_paypal_payment:
    resource: '@SisoPaypalPaymentBundle/Resources/config/routing.yml'
```

## Configure the paypal Express plugin

The configuration for payPal Express has to be setup for each siteaccess (at least when the backend is used).

![](attachments/23561073/23562127.png)

## How to get the API credentials

``` 
jms_payment_paypal:
    username: myusername
    password: maypassword
    signature: A5Va2XJid60kg21ddddddxKbSykH4i.ddsdsd-332yT0G8z8LrvNPl1
    debug: true
```

Login and navigate to "All Tools"

![](attachments/23561073/23563062.png)

Select the tool "API Access"

![](attachments/23561073/23563063.png)

Choose "Classic (NVP-)API integration"

![](attachments/23561073/23563060.png)

This page will list the necessary values.

![](attachments/23561073/23563061.png)

## Attachments:

![](images/icons/bullet_blue.gif) [image2018-4-19\_10-16-41.png](attachments/23561073/23563062.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-19\_10-25-2.png](attachments/23561073/23563063.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-19\_10-27-12.png](attachments/23561073/23563060.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-4-19\_10-30-18.png](attachments/23561073/23563061.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2018-11-26\_12-0-39.png](attachments/23561073/23562127.png) (image/png)  
