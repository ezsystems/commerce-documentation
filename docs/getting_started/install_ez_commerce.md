# Install eZ Commerce

!!! note

    Installation for production is only supported on Linux.

    To install eZ Commerce for development on macOS or Windows,
    see the instruction to [Install eZ Platform on macOS or Windows](https://doc.ezplatform.com/en/latest/community_resources/installing-on-mac-os-and-windows/).

## Prepare work environment

To install eZ Commerce you need a stack with your operating system, MySQL and PHP.

You can install it by following your favorite tutorial, for example: [Install LAMP stack on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-ubuntu-18-04).

Additionally, you need [Node.js](https://nodejs.org/en/) and [Yarn](https://yarnpkg.com/lang/en/docs/install/#debian-stable) for asset management.

[For production](#prepare-installation-for-production) you also need Apache or nginx as the HTTP server (Apache is used as an example below).

You also need `git` for version control.

To use search in the front end, you need to have [Solr installed](#install-solr).

Before getting started, make sure you review the [requirements](https://doc.ezplatform.com/en/latest/getting_started/requirements/) page to see the systems we support and use for testing.

## Get Composer

Install a recent stable version of Composer, the PHP command line dependency manager.
Use the package manager for your Linux distribution. For example on Ubuntu:

``` bash
apt-get install composer
```

To verify you got a recent stable version of Composer, you can run:

``` bash
composer -V
```

!!! tip "Install Composer locally"

    If you want to install Composer inside your project root directory only,
    run the following command in the terminal:

    ``` bash
    php -r "readfile('https://getcomposer.org/installer');" | php
    ```

    After this you need to replace `composer` with `php -d memory_limit=-1 composer.phar` in all commands below.

## Set up authentication tokens

See [Set up authentication tokens](https://doc.ezplatform.com/en/latest/getting_started/install_ez_enterprise/#set-up-authentication-tokens)
in eZ Platform installation to learn how to authenticate yourself for commercial packages.

## Create project

In order to install a new project using `composer create-project` to get latest version of eZ Commerce,
you'll need to tell Composer which token to use before the project folder is created in the first place.
This can be done in the following way:

``` bash
COMPOSER_AUTH='{"http-basic":{"updates.ez.no":{"username":"<installation-key>","password":"<token-password>"}}}' composer create-project --keep-vcs --repository=https://updates.ez.no/bul_com/ ezsystems/ezcommerce my-new-com-project
```

!!! tip "Usage of authentication token with `composer create-project`"

    If you have several projects set up on your machine,
    they should all use different tokens set in `auth.json` file in project directory.

!!! note "Moving from trial"

    If you started with a trial installation, you should [adjust the channel(s)](#edit-composerjson) you use in order to get software under [BUL license instead of a TTL license](https://ibexa.co/About-our-Software/Licenses-and-agreements/).

## Edit composer.json

Edit `composer.json` in your project root and change the URL defined in the `repositories` section to `https://updates.ez.no/bul_com/`.
Once that is done, you can execute `composer update` to get packages with the correct license.

## Change installation parameters

At this point you can configure your database via the `DATABASE_URL` in the `.env` file:
`DATABASE_URL=mysql://user:password@host:port/name`.

Choose a [secret](http://symfony.com/doc/5.0/reference/configuration/framework.html#secret)
and provide it in the `APP_SECRET` parameter in `.env`.
It should be a random string, made up of up to 32 characters, numbers, and symbols.
This is used by Symfony when generating [CSRF tokens](https://symfony.com/doc/5.0/security/csrf.html),
[encrypting cookies](http://symfony.com/doc/5.0/cookbook/security/remember_me.html),
and for creating signed URIs when using [ESI (Edge Side Includes)](https://symfony.com/doc/5.0/http_cache/esi.html).

Alternatively, you can also change individual installation parameters in `.env`.

!!! tip

    It is recommended to store the database credentials in your `.env.local` file and not commit it to the Version Control System.

The configuration requires providing the following parameters:

- `DATABASE_USER`
- `DATABASE_PASSWORD`
- `DATABASE_NAME`
- `DATABASE_HOST`
- `DATABASE_PORT`
- `DATABASE_PLATFORM` —  prefix for distinguishing the database you are connecting to (e.g. `mysql` or `pgsql`)
- `DATABASE_DRIVER` — driver used by Doctrine to connect to the database (e.g. `pdo_mysql` or `pdo_pgsql`)

!!! note "JMS payment secret"
    
    To provide the `JMS_PAYMENT_SECRET` secret for the payment system, run `./vendor/defuse/php-encryption/bin/generate-defuse-key`
    and use the generated secret. 

## Install Solr

eZ Commerce requires Solr as search engine. The demo provides a Solr installation which has to be started:

TODO

## Create database

!!! tip

    You can omit this step. If you do not create a database now, it will be created automatically in the next step.

To manually create a database, ensure that you [changed the installation parameters](#change-installation-parameters), then run the following Symfony command:

``` bash
php ./bin/console doctrine:database:create
```

## Install eZ Platform

Install eZ Platform with:

``` bash
composer ezplatform-install
```

This command will also create a database, if you had not created it earlier.
Before executing it make sure that the database user has sufficient permissions.

If Composer asks for your token, you must log in to your GitHub account and generate a new token
(edit your profile, go to Developer settings > Personal access tokens and Generate new token with default settings).
This operation is performed only once when you install eZ Commerce for the first time.

!!! tip "Enabling Link manager"

    To make use of [Link Manager](../guide/url_management.md), you need to [set up cron](../guide/url_management.md#enable-automatic-url-validation).

## Prepare installation for production

To use eZ Commerce with an HTTP server, you need to [set up directory permissions](#set-up-permissions) and [prepare a virtual host](#set-up-virtual-host).

### Set up permissions

For development needs, the web user can be made the owner of all your files (for example with the `www-data` web user):

``` bash
chown -R www-data:www-data <your installation directory>
```

Directories `var` and `public/var` need to be writable by CLI and web server user.
Future files and directories created by these two users will need to inherit those permissions.

!!! caution

    For security reasons, in production web server should not have write access to other directories than `var`. Skip the step above and follow the link below for production needs instead.

    You must also make sure that the web server cannot interpret files in the `var` directory through PHP. To do so, follow the instructions on [setting up a virtual host below](#set-up-virtual-host).

To set up permissions for production, it is recommended to use an ACL (Access Control List).
See [Setting up or Fixing File Permissions](http://symfony.com/doc/5.0/setup/file_permissions.html) in Symfony documentation
for information on how to do it on different systems.

### Set up virtual host

#### Option A: Scripted configuration

Use the included shell script: `/<your installation directory>/bin/vhost.sh` to generate a ready to use `.conf` file.
Check out the source of `vhost.sh` to see the options provided.

#### Option B: Manual configuration

Copy `/<your installation directory>/doc/apache2/vhost.template` to `/etc/apache2/sites-available` as a `.conf` file.

Modify it to fit your project.

Specify `/<your installation directory>/public` as the `DocumentRoot` and `Directory`.
Uncomment the line that starts with `#if [SYMFONY_ENV]` and set the value to `prod` or `dev`,
depending on the environment you want:

```
SetEnvIf Request_URI ".*" SYMFONY_ENV=prod
```

#### Enable virtual host

When the virtual host file is ready, enable the virtual host and disable the default:

``` bash
a2ensite ezplatform
a2dissite 000-default.conf
```

Finally, restart the Apache server. The command may vary depending on your Linux distribution. For example of Ubuntu use:

``` bash
service apache2 restart
```

Open your project in the browser and you should see the welcome page.

!!! tip "eZ Launchpad for quick deployment"

    If you want to get your eZ Commerce installation up and running quickly,
    you can use Docker-based [eZ Launchpad](https://ezsystems.github.io/launchpad/), supported by the eZ Community,
    which takes care of the whole setup for you.
