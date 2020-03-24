# Step 1 - Install the DigitalProductBundle

The repo <https://github.com/silversolutions/digitalproductsbundle.git> contains the required code for the tutorial:

Add the gihub repo to your composer.json

``` json
"repositories": [
{
 "type": "vcs",
 "url": "https://github.com/silversolutions/digitalproductsbundle.git"
 }
]
```

Install the bundle:

`php -d memory\_limit=-1  composer.phar require silversolutions/digitalproducts dev-master`
