# Installation and configuration FAQ

## What configuration is required in order to communicate with ERP?

In order to communicate with ERP you have to make sure, that:

- You have configured the correct webconnector URL: `webconnector_url: ''`
- You have created the symlinks for the mapping

``` bash
cd app/Resources
ln -s ../../vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase
ln -s ../../src/<Company>/Bundle/ProjectBundle/Resources/xsl
```

- Make sure that you have the correct version of `vendor/moyarada`.

## Images in shop are not converted. What should I check?

Make sure that you have set up correct rights for the image folder:

``` bash
sudo chmod -R g+w web/var/ecommerce/storage/
```

## I am getting an error after installation

`[Defuse\Crypto\Exception\BadFormatException] Encoding::hexToBin() input is not a hex string`

You need to generate a secret using `./vendor/defuse/php-encryption/bin/generate-defuse-key`  
You can configure it in `parameters.yml`:

``` 
env(JMS_PAYMENT_SECRET): 'def000004cb9c9f5edb77182df64b3d572162a47ec971a9a8beb00459b49fd9a1f9df6991ffc817c8585f59b8c5a032b796ab520eae126c77d8a304b36af0c9acdbfa9b9'
```
