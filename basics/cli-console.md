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
  make
    make:controller  Create a controller
    make:key         Generate an encryption key for the config file
    make:model       Create a model
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

update shows?
