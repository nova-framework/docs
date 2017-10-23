Nova comes with a command line utility called **Forge** located in the root. It supports creating controllers, models, modules and more from the command line.

To use it navigate to the project in your command line/terminal then type **php forge** followed by the command.

Typing just **'php forge'** will give this output:

```php
Nova Framework version 3.78.20

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
      --env[=ENV]       The environment the command should run under.
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  clear-compiled            Remove the compiled class file
  down                      Put the Application into Maintenance Mode
  env                       Display the current Framework Environment
  hello                     Display a Hello World message
  help                      Displays help for a command
  list                      Lists commands
  migrate                   Run the database migrations
  optimize                  Optimize the Framework for better performance
  routes                    List all registered routes
  serve                     Serve the Application on the PHP development server
  up                        Bring the Application out of Maintenance Mode
 auth
  auth:clear-reminders      Flush expired reminders.
  auth:reminders-table      Create a migration for the password reminders table
 cache
  cache:clear               Flush the Application cache
  cache:forget              Remove an item from the cache
  cache:table               Create a migration for the Cache database table
 db
  db:backup                 Backup the default database to `app/Database/Backup`
  db:restore                Restore a dump from `app/Database/Backup`
  db:seed                   Seed the database with records
 key
  key:generate              Set the Application Key
 mailer
  mailer:spool:clear        Clear the Mailer Spool queue of the messages failed to be sent
  mailer:spool:send         Send the messages queued in the Mailer Spool queue
  mailer:spool:table        Create a migration for the Mailer Spool database table
 make
  make:console              Create a new Forge command
  make:controller           Create a new Controller class
  make:event                Create a new Event class
  make:listener             Create a new Event Listener class
  make:migration            Create a new migration file
  make:model                Create a new ORM Model class
  make:module               Create a new Application Module
  make:module:console       Create a new Module Forge command
  make:module:controller    Create a new Module Controller class
  make:module:migration     Create a new Module Migration file
  make:module:model         Create a new Module Model class
  make:module:notification  Create a new Module Notification class
  make:module:policy        Create a new Module Policy class
  make:module:provider      Create a new Module Service Provider class
  make:module:seeder        Create a new Module Seeder class
  make:notification         Create a new Notification class
  make:policy               Create a new Policy class
  make:provider             Create a new Service Provider class
  make:seeder               Create a new Database Seeder class
 migrate
  migrate:install           Create the migration repository
  migrate:refresh           Reset and re-run all migrations
  migrate:reset             Rollback all database migrations
  migrate:rollback          Rollback the last database migration
  migrate:status            Show the status of each migration
 module
  module:list               List all Application Modules
  module:migrate            Run the database migrations for a specific or all modules
  module:migrate:refresh    Reset and re-run all migrations for a specific or all modules
  module:migrate:reset      Rollback all database migrations for a specific or all modules
  module:migrate:rollback   Rollback the last database migrations for a specific or all modules
  module:migrate:status     Show the status of each migration
  module:optimize           Optimize the module cache for better performance
  module:seed               Seed the database with records for a specific or all modules
 notifications
  notifications:table       Create a migration for the notifications table
 schedule
  schedule:run              Run the scheduled commands
 session
  session:table             Create a migration for the Session database table
 view
  view:clear                Clear all compiled View files
```
## remove cache files
To remove the files in storage/cache:

```php
php forge clear:cache
```

## remove the log files
To remove the files in storage/logs:

```php
php forge clear:logs
```

## remove the session files
To remove the files in storage/sessions:

```php
php forge clear:sessions
```

## remove the view cache files
To remove the files in storage/views:

```php
php forge clear:views
```

## Creating a controller
To create a controller type make:controller followed by the names of the methods to be created:

```php
php forge make:controller lists index add edit view delete
```

This will create a controller called Lists with 5 methods, additionally a Lists folder will be created in the views folder along with 5 files, one for each method. The files will be empty.

## Creating a model
To create a model type model followed by the names of the methods to be created:

```php
php forge make:model lists getAll add edit delete
```

This will create a controller called Lists with 4 methods.

## Generate an Encryption Key
To generate an encryption key for **app/Config/App.php**, type in the following:

```php
php forge make:key
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
php -d register_argc_argv=On forge/forge schedule:run
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

```php
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

This is configured by `spool.messageLimit` in `app/Config/Mail.php`
The value of 10 have the sense of sending max 10 messages per cycle (of one minute, usually)

So, this is max 600 messages per hour.

BUT, because the messages are queued, the process will continue until all messages from the Spool queue will be sent, but no more than 10 messages per minute.

### The disadvantage of this design
Email sending is delayed. For example, imagine that 100 users ask for a password recovery at the same time.

Depending on the real order of receiving the requests, can be up to 10 minutes until a email for password recovery will be sent to a particular user, who's unlucky to be the last one.
