
# Modules

Modules are like mini applications. A module can be standalone section or add integration functionality to Nova.

## Sample Modules:

Nova comes with a few sample modules, these are for showing examples of what can be created with modules as such these are not officially supported modules, having said that they are kept up to date with any changes required.

### Demos

Demos's purpose is to demonstrate some of Nova's features, looking inside **app/Modules/Demos/Routes.php** are these routes:

Each route is a different demo for instance going to **/demo/database** (requires a database being connected in **app/Confog/Database.php**)
will show a sample of database interactions.

**/demo/modules** will display a list of all active modules.

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

### Files

This is a secure filemanager using [elfFinder](https://studio-42.github.io/elFinder/), to access it you must first login by going to **/admin** which uses the system module (see below)
This module is a full featured file manager capable of working with local files as well as third party adapters such as FTP, AW3 or Dropbox

All files are stored above the webroot making it very secure.

###

>Import the sql files from /scripts first.

The system module offers an admin area out of the box by going to **/admin** and logging in with these details:

username: admin<br>
password: admin

This demonstrates a way to have a backend area out of a module, it's recommended looking through these modules for greater understanding of their usage.

### Users

Nova comes with a groups of users, each user will have a role ie administrator. This module offers CRUD functions for adding, editing and deleting users and their roles.

# Creating a module

Modules consist of files and folders which you use is up to you, a module can have few files to be up and running or can host many folders.

Here are the common files and folders used:

```php
Assets/
Controllers/
Language/
Models/
Providers/
Views/
Bootstrap.php
Config.php
Events.php
Filters.php
Routes.php
```

A module uses the same types of files as the main controllers do so each module can have their own config settings, routes and events.

Additional folders can be created with classes inside as long as they use the correct namespace ie for a class in **app/Modules/Contacts/Lib/Exchange.php** would have a namespace:

```php
namespace App\Modules\Contacts\Lib;
```

Each module should have a Routes.php to control what url loads up a module controller.


## To create a module

it's recommended to use the terminal command:

```php
php forge make:module NameOfModule
```

Then enter the name followed by the slug which should be lowercase with no spaces. The slug is a url friendly name of the module.

Further forge commands can be used to make a controller and view.

## To create a module controller:

```php
php forge make:module:controller modulename controllername
```

change modulename to match the slug of the module ie `blog` and controllername should be the name of the controller wanted.

## To create a module model:

```php
php forge make:module:model modulename modelname
```

change modulename to match the slug of the module ie `blog` and modelname should be the name of the model wanted.
