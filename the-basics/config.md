Settings for the framework setup in app/Config.php

Set the timezone to your local

````
date_default_timezone_set('Europe/London');
````

Next set the application web URL, Once the SITEURL is set it can be used to get the application address.

````
define('SITEURL', 'http://example.com/');
````

Set the base url if on the root of a domain it's a single otherwise it's /foldername/

````
define('DIR', '/');
````

If using a database the database credentials will need adding:

````
define('DB_TYPE', 'mysql');
define('DB_HOST', 'localhost');
define('DB_NAME', 'database name');
define('DB_USER', 'root');
define('DB_PASS', 'root');
define('PREFIX', 'nova_');
````

The prefix is optional but highly recommended, it's very useful when sharing databases with other applications, to avoid conflicts. The prefix should be the starting pattern of all table names like nova_users

Nova provides a session helper class, in order to avoid session conflicts a prefix is used.

````
define('SESSION_PREFIX', 'nova_');
````

The following tells the framework what theme folder to use for views

````
define('TEMPLATE','default');
````

By default routing uses named routes that you set in app/Routes.php, optionally classic routes can be used (controller/method/params) to use classic routes comment out the top Router and use ClassicRouter

````
// Default Routing
define('APPROUTER', '\Core\Router');

// Classic Routing
// define('APPROUTER', '\Core\ClassicRouter');
````
