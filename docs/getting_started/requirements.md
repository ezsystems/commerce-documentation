# Requirements

eZ Commerce requires:

- An eZ Platform Version 2.x installation (requirements see <https://doc.ezplatform.com/en/2.2/getting_started/requirements_and_system_configuration/>). Please check the release notes to get the exact eZ Platform requirements  

!!! note

    Recommended PHP Version: 7.2

    Please note that the following PHP extensions are required

    - curl extension and SOAP extension
    - intl extension
    - xsl extension

- An eZ Commerce Enterprise contract is required
- Advanced version only: An ERP system supported by eZ Commerce and the Web-Connector 

## Requirements user (frontend)

|Requirement|Details|
|--- |--- |
|JavaScript|required for frontend use|
|Browsers|Depends on the design used for the project.</br>The standard eZ Commercesupports:</br>Internet Explorer 9,10,11 (IE8 should work as well)</br>All the other modern browsers like Chrome, Firefox, Safari, Opera|

## Requirements server

Hardware

eZ Commerce requires a dedicated server. Due to security reason a shared hosting is not recommended.

|Function|Small system|Medium system|
|---|---|---|
|CPU|4x min. 2 GHz|6x min. 3 GHz|
|RAM|8 GB DDR3|16GB RAM|
|Harddisc (Raid 1 )|2x 500 GB|2x 500 GB|
|IP|1 fixed IP|1 fixed IP|
|Firewall|Port 80, ssh and 443|Port 80, ssh and 443|
|Access|ssh|ssh|

Management by a hosting company

- Monitoring server
- Monitoring services
- security patches
- Backups
- Support if new modules are required
- Support in case of issues
- Reporting

### Configuration notes

!!! note

    Please don´t store session as a file on the file system. This might cause locking problems.

    Details see [Session handling](../enhanced_configuration/configuration/session_handling.md)

Please check if your webserver configuration (or varnish) blocks cookies which are not used in the application. 

The reason is that additional cookies might influence the caching behaviour in the backend. Some parts of the site are cached per session (e.g. the basket preview).  It uses an ESI block which uses a cache by Cookie.

The implementation of eZ Platform / the FOS bundle is building a hash using all cookies.  

For Apache you could use mod\_headers with an additional line for your vhost (this is an example, for apache v2.2.4+):

``` 
RequestHeader edit Cookie "^(.*?)YOUR_COOKIE_NAME=.*?(?:$|;)(.*)$" $1$2
```

For nginx in you vhost configuration:

``` 
set $http_cookie_updated $http_cookie;
if ($http_cookie ~ "(.*)(^|;)\s*YOUR_COOKIE_NAME=[^;]+(?:;|$)(.*)") {
  set $http_cookie_updated $1$2$3;
}
proxy_set_header Cookie $http_cookie_updated;
```

### Network

| Direction              | Ports  | Desc                                                                                               |
| ---------------------- | ------ | -------------------------------------------------------------------------------------------------- |
| Internet -> Shop      | 80/443 | Traffic customers                                                                                  |
| Shop -> Mail-server   | 25     | smtp for sending emails                                                                            |
| Shop -> Web-Connector | 1443   | Advanced version only Connection to the Web-Connector PC installed in the Intranet of the Customer |

### Software

In addition the the requirements for eZ Platform  the following software needs to be installed:

#### Node.js / NPM

Node.js (<https://nodejs.org/>) with NPM (Node Package Manager)

NPM Packages:

- <https://github.com/fmarcia/UglifyCSS> - installed globally or locally (app/Resources)
- <https://github.com/mishoo/UglifyJS> - version 2.4.15 (this seems to work best with current Assetic - needs to be checked for every project) installed globally or locally (app/Resources)  

```
// install globally
npm install uglifycss -g
npm install uglify-js@2.4.15 -g
 
// install locally
cd app/Resources/
npm install uglifycss
npm install uglify-js@2.4.15 
```

#### MySQL

Please make sure that the following option is set (e.g. /etc/mysql/my.cnf): 

``` 
[mysqld]
# Only allow connections from localhost
sql_mode = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
```

If you encounter issues in the backend with missing chart data in the cockpit please check this setting and restart mysqld\!
