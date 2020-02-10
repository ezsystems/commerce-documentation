#  Installation & configuration FAQs 

![](images/icons/grey_arrow_down.png)Advanced version only Which configuration is required in order to communicate with ERP?

In order to communicate with ERP you have to make sure, that:

  - You configured the correct webconnector url:

    ``` 
    webconnector_url: ''
    ```

  - You have created the symlinks for the mapping

    ``` 
    cd app/Resources
    ln -s ../../vendor/silversolutions/silver.e-shop/src/Silversolutions/Bundle/EshopBundle/Resources/xslbase
    ln -s ../../src/<Company>/Bundle/ProjectBundle/Resources/xsl
    ```

  - Make sure that you have the correct version of vendor/moyarada (see below)

![](images/icons/grey_arrow_down.png)The images in shop are not converted. What should i check?
Make sure that you have setup correct rights for the image folder:

``` 
sudo chmod -R g+w web/var/ecommerce/storage/
```

![](images/icons/grey_arrow_down.png)After the installation i am getting the error: \[Defuse\\Crypto\\Exception\\BadFormatException\] Encoding::hexToBin() input is not a hex string.
You need to generate a secret using ./vendor/defuse/php-encryption/bin/generate-defuse-key  
You can configure it in parameters.yml:

``` 
env(JMS_PAYMENT_SECRET): 'def000004cb9c9f5edb77182df64b3d572162a47ec971a9a8beb00459b49fd9a1f9df6991ffc817c8585f59b8c5a032b796ab520eae126c77d8a304b36af0c9acdbfa9b9'
```
