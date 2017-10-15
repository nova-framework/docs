New to 3 is a command line utility aptly named **Nova** located in the root. It currently supports creating controllers and models from the command line.

To use it navigate to the project in your command line/terminal then type **php nova** followed by the command.

Typing just **'php nova'** will give this output:

```php
Nova Framework Command Line Interface for v3.0 version 1.3.0

Usage:
    command [options] [arguments]

Options:
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  help               Displays help for a command
  list               Lists commands
  clear
    clear:cache      Clears the cache folder
    clear:logs       Clears the log files
    clear:sessions   Clears the sessions folder
    clear:views      Clears the view cache folder
  make
    make:controller  Create a controller
    make:key         Generate an encryption key for the config file
    make:model       Create a model
```
## remove cache files
To remove the files in storage/cache:

```php
php nova clear:cache
```

## remove the log files
To remove the files in storage/logs:

```php
php nova clear:logs
```

## remove the session files
To remove the files in storage/sessions:

```php
php nova clear:sessions
```

## remove the view cache files
To remove the files in storage/views:

```php
php nova clear:views
```

## Creating a controller
To create a controller type make:controller followed by the names of the methods to be created:

```php
php nova make:controller lists index add edit view delete
```

This will create a controller called Lists with 5 methods, additionally a Lists folder will be created in the views folder along with 5 files, one for each method. The files will be empty.

## Creating a model
To create a model type model followed by the names of the methods to be created:

```php
php nova make:model lists getAll add edit delete
```

This will create a controller called Lists with 4 methods.

## Generate an Encryption Key
To generate an encryption key for **app/Config/App.php**, type in the following:

```php
php nova make:key
```

This will generate a 32 character alpha-numeric key.

## Backup Database

> added in 3.78.8

```php
forge db::backup
```

This generates a full sql backup in `App\Database\Backup`

If you get the response:

```php
Database backup failed. sh: /usr/bin/mysqldump: No such file or directory
```

Update the mysql path in  `App\Config\Database.php` to include local:

```php
'mysql' => array(
    'dumpCommandPath'    => '/usr/local/bin/mysqldump',
    'restoreCommandPath' => '/usr/local/bin/mysql',
),
```

To run this automatically via a cron job by calling:

```php
<full path to PHP> <full path to forge> db:backup
```

## Schedule:

Setup a schedule in `app/Console.php`:

```php
Schedule::command('db:backup')->daily();
```

Then every day at 12am, your backup will be generated automatically.

To call the cron

```php
 * * * * /path/to/php /path/to/forge schedule:run >> /dev/null 2>&1
```

>note the /dev/null is to stop any output being created on your server.

see all commands at [system/Event.php](https://github.com/nova-framework/system/blob/3.0/src/Console/Scheduling/Event.php)

Servers with `register_argc_argv` set to off will generate an error, to fix set `register_argc_argv` to on for the server in php.ini or pass it in directly and switch it on.

```php
php -d register_argc_argv=On nova/forge schedule:run
```

example way to call a controller method

```php
Schedule::call('App\Modules\Customers\Controllers\Admin\Customers@cron')->everyMinute();
```

## Mail Spooling
There is the Mailer::send(), which is used to send emails now there is a new method `Mailer::queue()`, with the same syntax.

This places the message in the Spool queue to be sent later  instead to send it directly.

To send the messages stored in the Spool, can be done by a CRON command which call directly the forge:

```php
mailer:spool:send
```

OR in the default given setup of Nova applications, to use the Console Scheduling.

The command by default is in `app/Console.php`

```
 //Schedule the Mailer Spool queue flushing.

Schedule::command('mailer:spool:send')->everyMinute();
```

Then if you run

```php
forge schedule:run
```

It will be executed.

### What advantages does Mailer Spool provide?

Email sending could be slow, which can have a negative impact on the speed of receiving the next/response page.

Using the Spool queuing to send messages is very fast, the site interface speed is not affected.

Another advantage of using the Spool queuing, because of its precise cycling nature, allows to configure a limitation of the number of messages sent per minute, very useful on most hosted servers, which have a limitation of the number of messages sent.

This is configured by spool.messageLimit on app/Config/Mail.php
The value of 10 have the sense of sending max 10 messages per cycle (of one minute, usually)

So, this is max 600 messages per hour.

BUT, because the messages are queued, the process will continue until all messages from the Spool queue will be sent, but no more than 10 messages per minute.

The disadvantage of this design is that there can appear delays on the emails sending.
For example, imagine that 100 users ask for a password recovery in the same minute.

Yet, depending on the real order of receiving the requests, can be up to 10 minutes until a email for password recovery will be sent to a particular user, who's unlucky to be the last one.

`dump($item)`

dump outputs the variable without exiting the application.
