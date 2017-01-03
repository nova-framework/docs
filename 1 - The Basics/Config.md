Settings for the framework setup in app/Core/Config.php

Set the timezone to your local
```
date_default_timezone_set('Europe/London');
```

Next set the application web URL, Once the DIR is set it can be used to get the application address.
```
define('DIR','http://example.com/');
```

If using a database the database credentials will need adding:
```
define('DB_TYPE','mysql');
define('DB_HOST','localhost');
define('DB_NAME','database name');
define('DB_USER','root');
define('DB_PASS','root');
define('PREFIX','smvc_');
```

The prefix is optional but highly recommended, its very useful when sharing databases with other applications, to avoid conflicts. The prefix should be the starting patten of all table names like smvc_users

The framework provides a session helper class, in order to avoid session conflicts a prefix is used.
```
define('SESSION_PREFIX','smvc_');
```

The following tells the framework what theme folder to use for views

```
define('TEMPLATE','default');
```
