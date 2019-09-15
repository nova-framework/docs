Nova comes with a command line utility called **Forge** located in the root. Forge supports creating controllers, models, packages and more from the command line.

To use it navigate to the project in your command line/terminal then type **php forge** followed by the command.

Typing just **'php forge'** will give this output:

```php
Nova Framework 4.2.4

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
  clear-compiled             Remove the compiled class file
  down                       Put the Application into Maintenance Mode
  env                        Display the current Framework Environment
  help                       Displays help for a command
  list                       Lists commands
  migrate                    Run the database migrations
  optimize                   Optimize the Framework for better performance
  routes                     List all registered routes
  serve                      Serve the Application on the PHP development server
  up                         Bring the Application out of Maintenance Mode
 asset
  asset:link                 Create a symbolic link from "webroot/assets" to the "assets" folder
  asset:publish              Publish a package's assets to the public directory
 cache
  cache:clear                Flush the Application cache
  cache:forget               Remove an item from the cache
  cache:table                Create a migration for the Cache database table
 config
  config:publish             Publish a package's configuration to the application
 db
  db:seed                    Seed the database with records
 key
  key:generate               Set the Application Key
 language
  language:update            Update the Language files
 log
  log:clear                  Clear log files
 make
  make:console               Create a new Forge command
  make:controller            Create a new Controller class
  make:event                 Create a new Event class
  make:job                   Create a new Job class
  make:listener              Create a new Event Listener class
  make:middleware            Create a new Middleware class
  make:migration             Create a new migration file
  make:model                 Create a new ORM Model class
  make:notification          Create a new Notification class
  make:package               Create a new Package
  make:package:console       Create a new Package Forge Command class
  make:package:controller    Create a new Package Controller class
  make:package:event         Create a new Package Event class
  make:package:job           Create a new Package Job class
  make:package:listener      Create a new Package Event Listener class
  make:package:middleware    Create a new Package Middleware class
  make:package:migration     Create a new Package Migration file
  make:package:model         Create a new Package Model class
  make:package:notification  Create a new Package Notification class
  make:package:policy        Create a new Package Policy class
  make:package:provider      Create a new package Service Provider class
  make:package:request       Create a new Package Request class
  make:package:seeder        Create a new Package Seeder class
  make:policy                Create a new Policy class
  make:provider              Create a new Service Provider class
  make:request               Create a new Request class
  make:seeder                Create a new Database Seeder class
  make:shared                Create the standard Shared namespace
 migrate
  migrate:install            Create the migration repository
  migrate:refresh            Reset and re-run all migrations
  migrate:reset              Rollback all database migrations
  migrate:rollback           Rollback the last database migration
  migrate:status             Show the status of each migration
 notifications
  notifications:table        Create a migration for the notifications table
 package
  package:list               List all Framework Packages
  package:migrate            Run the database migrations for a specific or all Packages
  package:migrate:refresh    Reset and re-run all migrations for a specific or all Packages
  package:migrate:reset      Rollback all database migrations for a specific or all Packages
  package:migrate:rollback   Rollback the last database migrations for a specific or all Packages
  package:migrate:status     Show the status of each migration
  package:optimize           Optimize the packages cache for better performance
  package:seed               Seed the database with records for a specific or all Packages
 queue
  queue:failed               List all of the failed queue jobs
  queue:failed-table         Create a migration for the failed queue jobs database table
  queue:flush                Flush all of the failed queue jobs
  queue:forget               Delete a failed queue job
  queue:listen               Listen to a given queue
  queue:monitor              Monitor the Queue Worker execution
  queue:restart              Restart queue worker daemons after their current job
  queue:retry                Retry a failed queue job
  queue:subscribe            Subscribe a URL to an Iron.io push queue
  queue:table                Create a migration for the queue jobs database table
  queue:work                 Process the next job on a queue
 schedule
  schedule:finish            Handle the completion of a scheduled command
  schedule:run               Run the scheduled commands
 session
  session:table              Create a migration for the Session database table
 vendor
  vendor:publish             Publish any publishable assets from vendor packages
 view
  view:clear                 Clear all compiled View files
  view:publish               Publish a Package views to the application
```
## Down

Put Nova into Maintenance mode:

```php
php forge down
```

This produced the output of 'Service unavailable Be right back.' instead of loading pages. This is useful for when making upgrades.

To change what is displayed go to `app/Views/Errors/503.php`

## Display Environment

```php
php forge env
```

Displayed the current environment `Current Application Environment: local`

## Migrations

Migration (Covered further in the database/migrations section of the docs)

Creates a migrations table and also all tables defined within migration files in `app/Database/Migrations`

```php
php forge migrate
```

## Optimize

This command refreshed the system cache

```php
php forge optimize
```


## remove cache files
To remove the files in storage/cache:

```php
php forge cache:clear
```

## remove the log files
To remove the files in storage/logs:

```php
php forge log:clear
```

## remove the view cache files
To remove the files in storage/views:

```php
php forge view:clear
```

## Creating a controller
To create a controller type make:controller followed by the names of the methods to be created:

```php
php forge make:controller Posts
```

This will create a controller called Posts with 5 methods.

## Creating a model
To create a model type model followed by the names of the methods to be created:

```php
php forge make:model Post
```

This will create a model called Post.

## Generate an Encryption Key
To generate an encryption key for **app/Config/App.php**, type in the following:

```php
php forge key:generate
```

This will generate a 32 character alpha-numeric key.
