
# Modules

Modules are like mini applications. A module can be standalone section or add integration functionality to Nova. 
For instance Nova with a few sample modules:

## Demos

Demos's purpose is to demonstrate some of Nova's featues, looking inside app/Modules/Demos/Routes.php are these routes:

Each route is a different demo for instance going to /demo/database (requires a database being connected in **app/Confog/Database.php**)
will show a sample of database interactions.

/demo/modules will display a list of all active modules.

```php
Route::get('database', 'Demos@database');
Route::get('events',   'Demos@events');
Route::get('mailer',   'Demos@mailer');
Route::get('session',  'Demos@session');
Route::get('validate', 'Demos@validate');
Route::get('paginate', 'Demos@paginate');
Route::get('cache',    'Demos@cache');
Route::get('modules',  'Demos@modules');

Route::get('password/{password}', 'Demos@password');
```

## Files

This is a secure filemanager using elfFinder, go access it you must first login by going to /admin which uses the system module (see below)
This module is a full featured file manager capable of working with local files as well as third party adapters such as FTP, AW3 or Dropbox

All files are stored above the webroot making it very secure.

## System

## Users

Nova comes with a groups of users, each user will have a role ie administrator, edit, user this module is offers CRUD functions for adding, editing and deleting users and hteir roles.
