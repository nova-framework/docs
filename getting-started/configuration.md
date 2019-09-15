- [Config](#config)
- [App](#app)
- [Auth](#auth)
- [Cache](#cache)
- [Database](#database)
- [Languages](#languages)
- [Mail](#mail)
- [Packages](#packages)
- [Profiler](#profiler)
- [Routing](#routing)
- [Session](#session)
- [View](#view)

<a name='config'></a>
#Configuration

Settings for the framework setup in **app/Config.php** and **app/Config/**

Set the prefix for the database:


```php
/**
 * PREFER to be used in Database calls or storing Session data, default is 'nova_'
 */
define('PREFIX', 'nova_');
```

The prefix is optional but **highly recommended**, it's very useful when sharing databases with other applications, to avoid conflicts. The prefix should be the starting pattern of all table names like **nova_users**


<a name='app'></a>
#App


```php
'debug' => true, // When enabled the actual PHP errors will be shown.
```

Assign the site url

```php
'url'  => 'http://www.novaframework.dev/',
```

Assign the admin's email.

```php
'email' => 'admin@novaframework.dev',
```

Assign the site name

```php
'name' => 'Nova 4.2',
```

The name of default Theme or false for disabling the usage of Themes.

```php
'theme' => false,
```

Assign the default locale, used for translation.

```php
'locale' => 'en',
```

```php
/**
 * The default Timezone for your website.
 * http://www.php.net/manual/en/timezones.php
 */
'timezone' => 'Europe/London',
```

Assign encryption key, you can use `php nova make:key` in a terminal window/ssh or generate a key by going to http://novaframework.com/token-generator

```php
key' => 'SomeRandomStringThere_1234567890'
```

Set alias paths - these allow shorter access to the methods without having to call the full path in views.

```php
'aliases' => array(
    // The Support Classes.
    'Arr'           => 'Nova\Support\Arr',
    'Str'           => 'Nova\Support\Str',

    // The Database Seeder.
    'Seeder'        => 'Nova\Database\Seeder',

    // The Support Facades.
    'App'           => 'Nova\Support\Facades\App',
    'Asset'         => 'Nova\Support\Facades\Asset',
    'Auth'          => 'Nova\Support\Facades\Auth',
    'Broadcast'     => 'Nova\Support\Facades\Broadcast',
    'Bus'           => 'Nova\Support\Facades\Bus',
    'Cache'         => 'Nova\Support\Facades\Cache',
    'Config'        => 'Nova\Support\Facades\Config',
    'Cookie'        => 'Nova\Support\Facades\Cookie',
    'Crypt'         => 'Nova\Support\Facades\Crypt',
    'DB'            => 'Nova\Support\Facades\DB',
    'Event'         => 'Nova\Support\Facades\Event',
    'File'          => 'Nova\Support\Facades\File',
    'Forge'         => 'Nova\Support\Facades\Forge',
    'Gate'          => 'Nova\Support\Facades\Gate',
    'Hash'          => 'Nova\Support\Facades\Hash',
    'Input'         => 'Nova\Support\Facades\Input',
    'Language'      => 'Nova\Support\Facades\Language',
    'Mailer'        => 'Nova\Support\Facades\Mailer',
    'Notification'  => 'Nova\Support\Facades\Notification',
    'Queue'         => 'Nova\Support\Facades\Queue',
    'Redirect'      => 'Nova\Support\Facades\Redirect',
    'Redis'         => 'Nova\Support\Facades\Redis',
    'Request'       => 'Nova\Support\Facades\Request',
    'Response'      => 'Nova\Support\Facades\Response',
    'Route'         => 'Nova\Support\Facades\Route',
    'Schedule'      => 'Nova\Support\Facades\Schedule',
    'Schema'        => 'Nova\Support\Facades\Schema',
    'Session'       => 'Nova\Support\Facades\Session',
    'Validator'     => 'Nova\Support\Facades\Validator',
    'Log'           => 'Nova\Support\Facades\Log',
    'URL'           => 'Nova\Support\Facades\URL',
    'Template'      => 'Nova\Support\Facades\Template',
    'View'          => 'Nova\Support\Facades\View',
    'Package'       => 'Nova\Support\Facades\Package',
);
```

<a name='auth'></a>
#Auth

Configuration options for use within the Auth system.

Authentication Defaults, sets the guard and table for reminders.

```php
'defaults' => array(
    'guard'    => 'web',
    'reminder' => 'users',
),
```

Set authentication model

```php
'model' => 'App\Models\User'
```

Password Reminder Settings

```php
/*
| Here you may set the settings for password reminders, including a view
| that should be used as your password reminder e-mail. You will also
| be able to set the name of the table that holds the reset tokens.
|
| The "expire" time is the number of minutes that the reminder should be
| considered valid. This security feature keeps tokens short-lived so
| they have less time to be guessed. You may change this as needed.
|
*/
'reminders' => array(
    'users' => array(
        'provider' => 'users',
        'email'    => 'Emails/Auth/Reminder',
        'table'    => 'password_reminders',
        'expire'   => 60,
    ),
)
```

#cache

Set Default driver, options:
*. files
*. database
*. auto
*. apc
*. memcached
*. redis
*. array


```php
'driver'   =>  'files'
```


Default Memcache Server for all $cache

```php
'memcached' => array(
    array('host' => '127.0.0.1', 'port' => 11211, 'weight' => 100),
),
```

#Database

Config options:

Set PDO fetch style

```php
'fetch' => PDO::FETCH_CLASS
```

The Default Database Connection Name

```php
'default' => 'mysql'
```

Set connections, additional databases can be used by adding additional arrays:

```php
// The Database Connections.
'connections' => array(
    'sqlite' => array(
        'driver'    => 'sqlite',
        'database'  => BASEPATH .'storage' .DS .'database.sqlite',
        'prefix'    => '',
    ),
    'mysql' => array(
        'driver'    => 'mysql',
        'hostname'  => 'localhost',
        'database'  => 'nova',
        'username'  => 'nova',
        'password'  => 'password',
        'prefix'    => PREFIX,
        'charset'   => 'utf8',
        'collation' => 'utf8_general_ci',
    ),
    'pgsql' => array(
        'driver'   => 'pgsql',
        'host'     => 'localhost',
        'database' => 'nova',
        'username' => 'nova',
        'password' => 'password',
        'charset'  => 'utf8',
        'prefix'   => PREFIX,
        'schema'   => 'public',
    ),
),
```

<a name='languages'></a>
#Languages

Array of all supported languages


```php
return array(
    'cs' => array('info' => 'Czech',     'name' => 'čeština',    'locale' => 'cs_CZ', 'dir' => 'ltr'),
    'da' => array('info' => 'Danish',    'name' => 'Dansk',      'locale' => 'da_DK', 'dir' => 'ltr'),
    'de' => array('info' => 'German',    'name' => 'Deutsch',    'locale' => 'de_DE', 'dir' => 'ltr'),
    'en' => array('info' => 'English',   'name' => 'English',    'locale' => 'en_US', 'dir' => 'ltr'),
    'es' => array('info' => 'Spanish',   'name' => 'Español',    'locale' => 'es_ES', 'dir' => 'ltr'),
    'fa' => array('info' => 'Persian',   'name' => 'پارسی',      'locale' => 'fa_IR', 'dir' => 'rtl'),
    'fr' => array('info' => 'French',    'name' => 'Français',   'locale' => 'fr_FR', 'dir' => 'ltr'),
    'it' => array('info' => 'Italian',   'name' => 'italiano',   'locale' => 'it_IT', 'dir' => 'ltr'),
    'ja' => array('info' => 'Japanesse', 'name' => '日本語',      'locale' => 'ja_JA', 'dir' => 'ltr'),
    'nl' => array('info' => 'Dutch',     'name' => 'Nederlands', 'locale' => 'nl_NL', 'dir' => 'ltr'),
    'pl' => array('info' => 'Polish',    'name' => 'polski',     'locale' => 'pl_PL', 'dir' => 'ltr'),
    'ro' => array('info' => 'Romanian',  'name' => 'Română',     'locale' => 'ro_RO', 'dir' => 'ltr'),
    'ru' => array('info' => 'Russian',   'name' => 'ру́сский',    'locale' => 'ru_RU', 'dir' => 'ltr'),
);
```

<a name='mail'></a>
#Mail

Mail settings:

Set the mail driver, options are:
* smtp
* mail
* sendmail
* mailgun
* mandrill
* log
```php
'driver' => 'smtp',
```

Host, set the host when using smtp
```php
'host' => '',
```

Username, for smtp
```php
'username' => '',
```

Password, for smtp
```php
'password' => '',
```

Port, specify the outgoing port
```php
'port' => 587,
```

Set default from name and email address
```php
'from' => array(
    'address' => 'admin@novaframework.local',
    'name'    => 'The Nova Staff',
),
```

Set encryption
```php
'encryption' => 'tls',
```

Pretend, when set to true emails will be stored in a log file rather then being sent out
```php
'pretend' => true,
```


<a name='packages'></a>
#Packages

Set default author information
```php
'author' => array(
    'name'     => 'John Doe',
    'email'    => 'john.doe@novaframework.local',
    'homepage' => 'http://novaframework.local',
),
```

Set modules path and namespace
```php
'modules' => array(
    'path' => BASEPATH .'modules',
    'namespace' => 'Modules\\',
),
```

Set themes path and namespace
```php
'themes' => array(
    'path' => BASEPATH .'themes',
    'namespace' => 'Themes\\',
),
```

<a name='profiler'></a>
# Profiler

You can set these options to true, if you need help with debugging, by default database number of sql queries is logged in the profiler **seen at the bottom of the default theme**

Setup the Profiler configuration
```php
return array(
    'useForensics' => false,
    'withDatabase' => false,
);
```
When the debug setting is set in app/Config/App.php (on by default) the following metrics are shown on the default theme:

> Elapsed Time: 0.1120 sec | Memory Usage: 4.35MB | SQL: 0 queries | UMAX: 223

**Elapsed Time**
Total time executed in seconds

**Memory Usage**
Total memory in MB used

**SQL**
When withDatabase setting is true the total number of queries ran

**UMAX**
UMAX represents a estimation of the maximum number of pages served per second, you can use UMAX as a general speed evaluation on a reasonable server load. Bigger is the number given by UMAX, better is it.

**Routing Configuration**

Assets routing:


```php
return array(
    /*
     * The Asset Files Serving configuration.
     */
    'assets' => array(
        // The path to the asset files.
        'path' => BASEPATH .'assets',

        // The browser Cache Control options.
        'cache' => array(
            'ttl'          => 600,
            'maxAge'       => 10800,
            'sharedMaxAge' => 600,
        ),

        // The Valid Vendor Paths - be aware that improper configuration of the Valid Vendor Paths could introduce
        // severe security issues, try to limit the access to a precise area, where aren't present "unsafe" files.
        //
        // '/vendor/almasaeed2010/adminlte/dist/css/AdminLTE.min.css'
        //          ^____________________^____^____________________Those are the parts of path which are validated.
        //
        'paths' => array(
            
        ),
    ),
);
```

<a name='session'></a>
#Session

Set session options


```php
return array(
    'driver' => 'file', // The Session Driver used for storing Session data; supported: 'file', 'database' or 'cookie'.

    // The Database Session Driver configuration.
    'table'      => 'sessions', // The Database Table hosting the Session data.
    'connection' => null,       // The Database Connection name used by driver.

    // Session Lifetime.
    'lifetime'      => 180,     // Number of minutes the Session is allowed to remain idle before it expires.
    'expireOnClose' => false,   // If you want them to immediately expire on the browser closing, set that.

    // The File Session Driver configuration.
    'files'    => STORAGE_PATH .'framework' .DS .'sessions', // File Session Handler - where the Session files may be stored.

    'lottery' => array(2, 100), // Option used by the Garbage Collector, to remove the stalled Session files.

    // Cookie configuration.
    'cookie'  => PREFIX .'session',
    'path'    => '/',
    'domain'  => null,
    'secure'  => false,
);
```
