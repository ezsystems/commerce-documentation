# Assetic step by step with minify option

!!! caution

    If you are working all the time in development mode (especially on Windows) you don't have to install NodeJS and two modules. We found some problems with those modules (which are recommended by Symfony) on Windows machines. In that case you can skip points 1 to 3 and comment filters in your config.yml file.

    If you really want to make filters work on Windows machines you can use YUI Compressor which is not recommended by Symfony anymore but should work. To install YUI Compressor as your filters please follow the instructions from official Symfony documentation: http://symfony.com/doc/current/cookbook/assetic/yuicompressor.html. In that case you can skip steps 1-4.

1\.  Make sure you have Node.js and NPM (Node Package Manager) installed on your machine. Here's how you can check. 

**Run from your console / terminal**

``` 
node --version
 
# example output
# v0.10.26
```

**Run from your console / terminal**

``` 
npm --version
 
# example output
# 1.4.3
```

If you don't have Node.js installed please visit <http://nodejs.org/> and click this big green install button to download installer for your operating system.  
When installing nodejs use this apt-get (Ubuntu)

``` 
sudo apt-get install nodejs
```

2\.  Create **node\_modules** directory inside app**/Resources **

``` 
cd app/Resources
mkdir node_modules
```

3\.  Install UglifyJS and UglifyCSS modules (via command line like Terminal, iTerm, etc and run).

**UglifyJS and UglifyCSS**

``` 
# you are in app/Resources
cd node_modules
npm install uglify-js@2.4.15
npm install uglifycss
```

Depending on your operating system you might have to run above commands with super user privileges (sudo).  
  
If you will have some errors during the installation it's possible that you have no permissions (look above) or your Node.js and NPM is outdated. In that case you will have update those dependencies.   

If you installed uglify-js without version specified it has installed latest version which may cause problems with Assetic bundle (details [here](https://github.com/mishoo/UglifyJS2/issues/236#issuecomment-88047640)) . Please make sure to install version 2.4.15

4\.  Recommended config\*.yml configuration. The configuration is divided into different files. The reason is that usually in dev mode you just don't need all the filter to be applied and have all the files minified/concatenated/compressed. You want this to happen when you are in production mode.  

**ezpublish/config/config.yml**

``` 
# Assetic Configuration
assetic:
    debug:          %kernel.debug%
    use_controller: false
    bundles:        [ eZDemoBundle, SilversolutionsEshopBundle, SisoOrderHistoryBundle ]
    filters:
        cssrewrite:
            apply_to: "\.css$"
```

**ezpublish/config/config\_dev.yml**

``` 
# Assetic Configuration
assetic:
    use_controller: true
```

**ezpublish/config/config\_prod.yml**

``` 
# Enables assetic filter in production mode
assetic:
    filters:
        uglifycss:
            bin:  %kernel.root_dir%/Resources/node_modules/.bin/uglifycss
            node: %siso_eshop.nodejs%
            apply_to: "\.css$"
        uglifyjs:
            bin:  %kernel.root_dir%/Resources/node_modules/.bin/uglifyjs
            node: %siso_eshop.nodejs%
            apply_to: "\.js$"
```

As you can see we are using `%siso_eshop.nodejs%` placeholder which is should be defined in this `app/config/parameters.yml` file.

Here is most likely the right configuration for Unix based operating systems:

**ezpublish/config/parameters.yml**

``` yaml
...
siso_eshop.nodejs: /usr/bin/node # or /usr/local/bin/node
```

**If you don't know what the is the path to Node.js please run this command:**

```
which node
```

5\.  Define **stylesheets** and **javascripts** blocks in your template (usually: **pagelayout.html.twig)**. Make sure stylesheets block is defined in the \<header\> area, and javascripts before closing body tag \</body\>.

**CSS WVGW example**

``` 
{% block stylesheets %}
        {% stylesheets
        'bundles/wvgwproject/css/reset.css'
        'bundles/wvgwproject/css/base.css'
        'bundles/wvgwproject/css/960_12_col.css'
        'bundles/wvgwproject/css/project.css'
        'bundles/wvgwproject/css/print.css'
        'bundles/wvgwproject/css/responsive.css'
        'bundles/wvgwproject/css/responsive/storm.responsive.navigation.css'
        'bundles/wvgwproject/css/messages/storm.messages.css'
        'bundles/wvgwproject/css/tooltip/storm.tooltip.css'
        'bundles/wvgwproject/css/tabs/storm.tabs.css'
        'bundles/wvgwproject/css/slick/slick.css'
        'bundles/wvgwproject/override/css/*'
        %}
        <link rel="stylesheet" href="{{ asset_url }}" />
        {% endstylesheets %}
    {% endblock %}
```

**JS WVGW example**

``` 
{% block javascripts %}
    {% javascripts
    'bundles/wvgwproject/js/assets/head.min.js'
    'bundles/wvgwproject/js/assets/jquery/jquery-1.7.2.min.js'
    'bundles/wvgwproject/js/assets/vendor.js'
    'bundles/wvgwproject/js/assets/underscore-min.js'
    'bundles/wvgwproject/js/assets/backbone-min.js'
    'bundles/wvgwproject/js/translations/storm.translations.js'
    'bundles/wvgwproject/js/translations/languages/de.js'
    'bundles/wvgwproject/js/project/languages/de.js'
    'bundles/wvgwproject/js/phalanx/storm.phalanx.controller.js'
    'bundles/wvgwproject/js/phalanx/storm.phalanx.parser.js'
    'bundles/wvgwproject/js/phalanx/storm.phalanx.provider.js'
    'bundles/wvgwproject/js/phalanx/hoplite/price/price.js'
    'bundles/wvgwproject/js/phalanx/hoplite/basket/basket.js'
    'bundles/wvgwproject/js/phalanx/hoplite/list/list.js'
    'bundles/wvgwproject/js/phalanx/hoplite/sidebox/sidebox.js'
    'bundles/wvgwproject/js/phalanx/hoplite/checkout/checkout.js'
    'bundles/wvgwproject/js/phalanx/storm.phalanx.actions.js'
    'bundles/wvgwproject/js/messages/storm.messages.js'
    'bundles/wvgwproject/js/messages/storm.messages.actions.js'
    'bundles/wvgwproject/js/responsive/storm.responsive.navigation.js'
    'bundles/wvgwproject/js/responsive/storm.responsive.navigation.actions.js'
    'bundles/wvgwproject/js/plugins/slick/slick.min.js'
    'bundles/wvgwproject/js/plugins/tooltip/jquery.tooltip.min.js'
    'bundles/wvgwproject/js/tooltip/storm.tooltip.actions.js'
    'bundles/wvgwproject/js/project/jquery.autocomplete.js'
    'bundles/wvgwproject/js/project/main.js'
    %}
    <script type="text/javascript" src="{{ asset_url }}"></script>
    {% endjavascripts %}
{% endblock %}
```

6\.  Generate assets with help Assetic (via command line) 

**For dev mode**

``` bash
php bin/console assetic:dump
```

**For prod mode**

``` bash
php bin/console assetic:dump --env=prod --no-debug
```

If you get an error about missing source files during assetic dump, you might have outdated asset links. Try to run asset:install to solve this. Example:

``` bash
php bin/console assets:install --symlink --relative
```

**Important: **If you change any of your assets (CSS or JS file), you **have to run** above commands to regenerate the files.

This command might be helpful for everyone who is developing constantly in development mode like Front-end developers. It will watch for changes in files defined in your stylesheets and javascripts blocks and regenerate them automatically for you.

**For dev mode (watches for changes and generates automatically if any of assets is changed)**

``` bash
php bin/console assetic:dump --watch
```

## How Assetic works for different environments

Assetic is compressing the css and js files depending on the debug-flag of the Symfony kernel, which is set depending to the environment variable USE\_DEBUGGING.

Per default in the **dev** environment this variable is set to true. In **prod** environment this variable is not set, that´s why the files are compressed into one.

If you want to create your own environment, you can define this variable in the `index_<environment>.php` file (e.g. `index_stage.php`).

### Example for stage environment with caching and compressed assetic files

**index\_stage.php**

``` 
putenv( "ENVIRONMENT=stage" );
//just set up the caching, but don´t use debug
putenv("USE_DEBUGGING=false");
putenv("USE_HTTP_CACHE=true");
require "index.php"; 
```

### Example for stage environment without caching and without compressed assetic files

**index\_stage.php**

``` 
putenv("ENVIRONMENT=stage");
putenv("USE_DEBUGGING=true");
putenv("USE_HTTP_CACHE=false");
require "index.php";
```

**Useful links if you are interested in more details:**

- <http://symfony.com/doc/current/cookbook/assetic/asset_management.html>
- <http://symfony.com/doc/current/cookbook/assetic/uglifyjs.html>
- <http://symfony.com/doc/current/cookbook/assetic/apply_to_option.html>
