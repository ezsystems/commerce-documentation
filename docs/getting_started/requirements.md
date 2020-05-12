# Requirements

eZ Commerce requires the following for installation:

- An eZ Platform v2.5.x installation (see [Requirements](https://doc.ezplatform.com/en/2.5/getting_started/requirements_and_system_configuration/)).
- An eZ Commerce Enterprise contract
- To use ERP connection: an ERP system supported by eZ Commerce and the Web Connector 

Recommended PHP version is 7.2 with the following extensions:
`curl`, `SOAP`, `intl`, `xsl`.

## Client

|Requirement|Details|
|--- |--- |
|JavaScript|required for frontend use|
|Browsers|Depends on the design used for the project.</br>Internet Explorer 9, 10, 11</br>Chrome, Firefox, Safari, Opera|

## Server

### Hardware

eZ Commerce requires a dedicated server. Due to security reasons, shared hosting is not recommended.

|Function|Small system|Medium system|
|---|---|---|
|CPU|4x min. 2 GHz|6x min. 3 GHz|
|RAM|8 GB DDR3|16GB RAM|
|Harddisc (Raid 1 )|2x 500 GB|2x 500 GB|
|IP|1 fixed IP|1 fixed IP|
|Firewall|Port 80, ssh and 443|Port 80, ssh and 443|
|Access|ssh|ssh|

### Management by a hosting company

- Monitoring server
- Monitoring services
- Security patches
- Backups
- Support if new modules are required
- Support in case of issues
- Reporting

### Configuration notes

!!! caution

    Don't store session as a file on the file system. This might cause locking problems.

    For details see [Session handling](../guide/configuration/session_handling.md)

Check if your web server configuration (or Varnish) blocks unused cookies. 

The reason is that additional cookies might influence the caching behavior in the backend. Some parts of the site are cached per session (e.g. the basket preview). The site uses an ESI block which uses a Cache by Cookie.

The implementation of eZ Platform / the FOS bundle is building a hash using all cookies.  

For Apache you could use mod\_headers with an additional line for your vhost (this is an example, for apache v2.2.4+):

``` 
RequestHeader edit Cookie "^(.*?)YOUR_COOKIE_NAME=.*?(?:$|;)(.*)$" $1$2
```

For NGINX, in your vhost configuration:

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

In addition to the requirements for eZ Platform, the following software needs to be installed:

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

If you encounter issues in the backend with missing chart data in the cockpit, check this setting and restart mysqld.
