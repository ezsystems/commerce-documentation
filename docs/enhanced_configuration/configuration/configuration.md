#  Configuration 

eZ Commerce is a highly configurable software which allows us to implement different configurations and load different Bundles for different installations

## Http server configuration

Please check the documentation for eZ Platform for more details: <https://doc.ezplatform.com/en/latest/getting_started/install_ez_platform/#prepare-installation-for-production>

eZ Commerce requires one more rule in order to display images. The following examples shows the settings for apache2: 

``` 
RewriteRule ^/var/assets/.* - [L]
```

## Symfony configuration 

Basically, you can override any config file with special CLIENT-config files. 

## parameters.yml

The parameters.yml contains important settings for your Installation:

``` 
# This file is auto-generated during the composer install
parameters:
    env(SYMFONY_SECRET): ThisEzPlatformTokenIsNotSoSecret_PleaseChangeIt
    env(JMS_PAYMENT_SECRET): def00000706ea7318427e72fcea2c8ceb86773a4310e35119c48e3029196acfead1ba8cc898f48d1ef9cb3f7ebe191ab46eaf67ec94a2b6bd17c079ac7277de0175b9e3e
    env(DATABASE_DRIVER): pdo_mysql
    env(DATABASE_HOST): localhost
    env(DATABASE_PORT): null
    env(DATABASE_NAME): ezcommerce
    env(DATABASE_USER): root
    env(DATABASE_PASSWORD): mypassword
    env(DATABASE_SERVER_VERSION): 5.7.9
    siso_search.solr.host: localhost
    siso_search.solr.port: 8983
    siso_search.solr.core: collection1
    siso_search.solr.path: /solr
    env(SOLR_DSN): 'http://%siso_search.solr.host%:%siso_search.solr.port%%siso_search.solr.path%'
    env(SOLR_CORE): '%siso_search.solr.core%'
    siso_imagemagick_path: /usr/bin/convert
    siso_core.siteaccess_group: ezdemo_site_clean_group
    apache_tika_path: bin/tika-app-1.13.jar
```

## config.yml

eZ Commerce include 2 special config files "ecommerce.yml" and "ecommerce\_parameters.yml". 

``` 
imports:
    - { resource: default_parameters.yml }
    - { resource: parameters.yml }
    - { resource: security.yml }
    - { resource: env/generic.php }
    - { resource: env/platformsh.php }
    - { resource: services.yml }

    - { resource: ecommerce.yml }
    - { resource: ecommerce_parameters.yml }
```

## routing.yml

The following routs are required for eZ Commerce

``` 
silversolutions_eshop:
    resource: "@SilversolutionsEshopBundle/Resources/config/ses_routing.yml"
    prefix: /
silversolutions_datatypes:
    resource: '@SilversolutionsDatatypesBundle/Resources/config/routing.yml'
    prefix: /
silversolutions_tools:
    resource: '@SilversolutionsToolsBundle/Resources/config/routing.yml'
    prefix: /
silversolutions_translation:
    resource: '@SilversolutionsTranslationBundle/Resources/config/routing.yml'
    prefix: /

siso_checkout:
    resource: '@SisoCheckoutBundle/Resources/config/routing.yml'
    prefix: /
siso_comparison:
    resource: '@SisoComparisonBundle/Resources/config/routing.yml'
    prefix: /
siso_quick_order:
    resource: '@SisoQuickOrderBundle/Resources/config/routing.yml'
    prefix: /
siso_search:
    resource: '@SisoSearchBundle/Resources/config/routing.yml'
    prefix: /
siso_tools:
    resource: '@SisoToolsBundle/Resources/config/routing.yml'
    prefix: /
 
siso_shop_price_engine:
    resource: "@ShopPriceEnginePluginBundle/Resources/config/routing.yml"
    prefix: /
siso_payment:
    resource: '@SisoPaymentBundle/Resources/config/routing.yml'
siso_paypal_payment:
    resource: '@SisoPaypalPaymentBundle/Resources/config/routing.yml'
siso_orderhistory:
    resource: '@SisoOrderHistoryBundle/Resources/config/routing.yml'
    prefix: /
```

## Bundles

The method registerBundles() in our Kernel load different Bundles based on the environment.

In addition, you are able to load client-specific Bundles here:

``` 
  //custom bundles for clients
  switch ($this->getClient()) {
     case "demo":
         $bundles[] = new Silversolutions\Bundle\DemoBundle\SilversolutionsDemoBundle();
     }
```

The value that is considered here comes is passed from the vhost-config into the script. See above for details.
