- [Introduction] (#introduction)
- [Configuration] (#configuration)
- [Commands] (#commands)

<a name="introduction"></a>
## Introduction

Nova comes with a `forge` command to help you backup your application's database. Any backup dumps created with this command will be stored in `app/Database/Backup`.

The following databases are supported:

- mysql
- sqlite
- pgsql

<a name="configuration"></a>
## Configuration

You may change the configuration for the database backups in `app/Config/Database.php`

```php
/*
|--------------------------------------------------------------------------
| Database Backup
|--------------------------------------------------------------------------
|
*/

'backup' => array(
    // The path where database dumps are stored.
    'path'  => APPPATH .'Database' .DS .'Backup',

    // The paths to the MySQL tools used by Forge.
    'mysql' => array(
        'dumpCommandPath'    => '/usr/bin/mysqldump',
        'restoreCommandPath' => '/usr/bin/mysql',
    ),

    // Whether or not the dump file is compressed.
    'compress' => true,
),
```

<a name="commands"></a>
## Commands

#### Backup

Backup the database and create an SQL dump:

```shell
php forge db:backup {filename?} {--database=}
```

#### Restore

Restores an SQL dump:

```shell
php forge db:restore {dump?} {--database=}
```

Using `db:restore` without an `argument` and `option` will list all available dumps.