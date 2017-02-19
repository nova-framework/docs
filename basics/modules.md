
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

This is a secure filemanager using [elfFinder](https://studio-42.github.io/elFinder/), go access it you must first login by going to **/admin** which uses the system module (see below)
This module is a full featured file manager capable of working with local files as well as third party adapters such as FTP, AW3 or Dropbox

All files are stored above the webroot making it very secure.

### System

The system module offers an admin area out of the box by going to **/admin** and logging in with these details:

username: admin
password: admin

This demonstrates a way to have an backend area out of a module, it's recommended looking through these modules for greater understanding of their usage.

### Users

Nova comes with a groups of users, each user will have a role ie administrator, edit, user this module is offers CRUD functions for adding, editing and deleting users and their roles.

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

Each module should have a Routes.php to control what uri loads up a module controller.

Lets build a simple module, create a new folder inside app/Modules called Contacts create a Controllers and Views folder and Routes.php

inside Routes.php enter:

```php
Route::group(array('prefix' => 'contacts', 'namespace' => 'App\Modules\Contacts\Controllers'), function() {
    Route::get('/', 'Contacts@index');
});
```

This will create a route for the url **/contacts** which will then load **app/Modules/Contacts/Controllers/Contacts.php**

Then create a class called **Contacts.php** in **/app/Modules/Contacts/Controllers:**

```php
<?php
namespace App\Modules\Contacts\Controllers;

use App\Core\Controller;

class Contacts extends Controller
{
    public function index()
    {
        return $this->getView()->shares('title', 'Contacts');
    }
}
```

This is the bare minimum, set the namespace import the core controller, extend from that and create an index method that loads a view.

Next create this path: **/app/Modules/Contacts/Views/Contacts/Index.php** and place some content inside:

```php
Hello from the Contacts Module.
```

Next the module needs to be active to run go to **app/Config/Modules.php** add the following array to the list of existing ones:

```php
'contacts' => array(
    'namespace' => 'Contacts',
    'enabled'   => true,
    'order'     => 1001,
),
```

This will enable the module and tell Nova about it's existance and the order to load it in.

Now go to **/contacts** to see your view loaded! this is an extremely simple example but is enough to have a module working, the next steps would be to have a model and use a database. From here all the steps are the same as with the main files are documented in these docs.
