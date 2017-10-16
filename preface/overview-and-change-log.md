# Overview and Change Log

Nova Framework is a PHP 5.6+ MVC Framework. It's designed to be modular, allowing developers to build better and easy to maintain code with PHP.

To this end Nova does not come with lots of built in libraries / helpers or modules instead, it's left to the user to decide what they want to implement, this allows freedom to design and build how you see fit. Having said that there are a limited number provided.

This has been tested with php 5.6 and php 7.

Please report any bugs.

## 3.78.21

Added missing labels to system links via `Events.php` and updated css/js resources.

## 3.78.20

Improve the Account page from Frontend + new Chat module

## 3.78.19

Improve the AdminLite theme in the Frontend side

## 3.78.18

Improve the ACL infrastructure

Usually, the USER is considered the Creator/Root and get all permissions, by its particular ID.

This release honors this unwritten rule and add overall improvements.


## 3.78.17

Fixed ACL on Roles and Users

## 3.78.16

Move the application infrastructure to an ACL style Role/Permission

## 3.78.15

Implementation of support for the Database Spool in the Mailer Service

## 3.78.14

Consolidate the Authorization System (ACL) and implement an example into Users module

## 3.78.13

Update the RTL support on Themes and compatibility with the Kernels beyond 3.78.14

Continue the updates required by the usage of AdminLTE 2.4.0 and update the application for the latest 3.78.14 kernel (to be released).

Long story short, inside the Kernel, the Nova\Module namespace was renamed as Nova\Modules.

This require several Service Providers updates into `app/Config/App.php` and adjusting the ModuleServiceProvider from the shipped Modules, as:

```php
use Nova\Module\Support\Providers\ModuleServiceProvider as ServiceProvider;
```

should become:

```php
use Nova\Modules\Support\Providers\ModuleServiceProvider as ServiceProvider;
```

Those changes could be resolved also by a mass replace. To note: Nova\Module\ -> Nova\Modules\

## 3.78.12

Improve the Backend Layout into AdminLite theme

## 3.78.11

Update the AdminLite theme for AdminLTE 2.4.0

## 3.78.10

Improve the API support

Improve the API support, and implements the Token-based Authentication infrastructure needed by APIs.

Also, now after a positive check of the User authentication over a Guard, it is set as default for the further use by the auth Filter.

Finally, was added an example of API Route:

```php
Route::get('api/user', array('before' => 'auth:api', function (Request $request)
{
    return $request->user();
}));
```

## 3.78.9

Add an example for the Forge's Closure based commands

## 3.78.8

Improve the Route Filters and the Authentication infrastructure

Improve the Authentication infrastructure, including the associated Route Filters.

Notable, the logout Route is moved to the usage of POST HTTP method, and the CSRF check is applied to it.

Also, the auth.basic filter, used for basic HTTP authentication, now accepts an Authorization Guard parameter.

## 3.78.7

Implement the Mailer Spool queue and overall improvements

## 3.78.6

Implement on Shared the Database Backup support and a Router extension, update the Language files
Implemented on the Shared namespace the Database Backup support and a Router extension, for the command `Route::controller()`, which works as usual.

The Database Backup add two new Forge commands: db:backup and db:restore and save SQL dumps to the folder `app/Database/Backup`.

## 3.78.5

Improve the Modules and the Boot stage

## 3.78.4

Improve the Options handling, the Modules and implement the XL Layout on Bootstrap Theme
Notable, implements a large (1480px) layout for the Bootstrap theme, beyond the standard 1200px one.

## 3.78.3

Improve the Exceptions handling on Options loading

## 3.78.2

Overall improvements
Improved the Route Filters handling, and move the usage of Controllers side Filters to standard ones, on the example Backend.

In the same, the Route Filter role is improved and now it is used by default by the Controllers interested to restrict the access to administration side.

Also, the Roles checking is improved into `App\Models\User` Model.

Finally, overall small improvements are introduced.

## 3.78.1

Overall improvements
Introduced improvements on `App\Controllers\BaseController` and an improved options handling, which now are able to use the namespaced Config options, with activation by default of the Setting Page into example Backend.

Basically, the new Options handling permit also the usage of the Settings Page(s) into Modules, which use the Namespaced Config Items for their Config(uration).

Also, the improved `App\Controllers\BaseController` handle now entirely the (auto-)Theming, which permits to optimize the Nova\Routing\Controller for a better performance on the RESTful APIs.

Finally, to note that on the `App\Controllers\BaseController` the method `before()` was renamed as `initialize()`, for a better description of its very own role: to initialize the Controller instance before execution of the requested Action.

## 3.78.0

Implementation of the Database Migrations and Seeding

## 3.77.32

Add the App\Events\Event class and the App\Listeners namespace

## 3.77.31

Renamed the base Controllers for consistency, as following:

App\Core\Controller -> App\Controllers\BaseController
App\Core\BackendController -> App\Modules\System\Controllers\BaseController

Additionally, improvements are applied to App\Controllers\BaseController@getView

## 3.77.30

Code Cleanup

## 3.77.29

Remove the RainCaptcha support

Remove the RainCaptcha support, as it became obsolete.

To note that a small API change is introduced, but this is unavoidable, as the RainCaptcha reached its end of life.

## 3.77.28

Updated Danish and Russian translations

## 3.77.27

Update french translation

## 3.77.26

Adjust the reCaptcha config usage and language update

## 3.77.25

Improve the Layouts from Bootstrap Theme

## 3.77.24

Improve the Bootstrap Theme and move it to using the Template Engine

## 3.77.23

Improve the demo for Pagination

## 3.77.22

Improve App\Providers\ThemeServiceProvider

## 3.77.21

fixed modules demo and pagination in Demos module

## 3.77.20

Implement the Service Providers support for Themes

## 3.77.19

Overall improvements
Introduced a public method called `validate()` into base Controllers, this means any protected method called `validate()` will need to be renamed.

The suggested way of action is to rename those methods to `validator()`

## 3.77.18

Implementation of the Authorization Policies (ACL)

## 3.77.17

Update app/Boot/Start.php

## 3.77.16

Improve the Role-based Route Filter

## 3.77.15

Improve the Route Filters

## 3.77.14

Improve the Auth Filters

## 3.77.13

Update the App\Core\Controller

## 3.77.12

Improve the Route Filters

## 3.77.11

Improve the Route Filters and update the Auth configuration

## 3.77.10

Run 'forge module:optimize' on Composer install and update

## 3.77.9

Improve the Role-based Route Filter

## 3.77.8

Improve the Role-based Route Filter and update the Language files

## 3.77.7

Implementation of the Multi-Auth support

## 3.77.6

Update the composer.json to support 5.6+

Ideally php 7 and up should be used but 5.6 is supported.

## 3.77.4

Update the App\Core\BackendController

Renamed the method `setupLayout()` to `before()`

## 3.77.1

Module Improvements

Introduced a small correction default/Default/ into App\Modules\System\Controllers\Registrar and update the Modules support for compatibility with Kernel 3.77.1 and superior.

## 3.77.0

Replaced nova cli with forge cli and advanced modules


* Added a new file called app/Boot/Forge
* Removed the nova script
* Added forge script
* Forge side Service Providers was added in app/Config/App.php
* The Modules received a Config folder with associated files
* In the ModuleServiceProvider from every Module was added the package() and was removed the loadConfigFrom part
* Removed the nova cli and replaced with forge cli script to be similar to what 4 offers but note it does not h* all commands 4 has.
* Implemented Forge commands for Auth, Cache and Session Services. Updated the Modules and
* implement the Modules side Forge commands.

Modules now require service providers recommended to use forge to make a module.

```
php forge
```


## 3.76.5

Improve the Service Providers from Modules

Introduced a better and more versatile handling of the Modules via a triplet of Service Providers.

To note that this pull request require the Kernel 3.76.12, and it needs a fast merging and release.


## 3.76.4

Add support for Views Overrides in Themes and re-organize them

Re-organize the Themes, moving the Layouts to the Layouts folder and implements the support for the Views Override, which permit to override any View from Application or Modules per Theme base.

To note that now the Layouts should have the names capitalized/studly, for example: default.php -> Default.php

Also, the RTL Layouts should stay in the folder Layouts/RTL while having the same name as the base ones.


## 3.76.3

Renamed the Langauge folders to be uppercase

## 3.76.2

Improve the `nova` script, setup and bring up the Nova Application in the Console Environment.

## 3.76.1

Upgraded Nova CLI you can use the following Facades: App, File and Config. Also, are available the following functions: base_path(), app_path()

A new command have been added to clear out the view cache files:

```php
php nova clear:views
```

## 3.76.0

Introduce the native **Template Engine** which respond to View files with the extension `.tpl` or `.ntp and update the Views Service.

To note the that the `Layout Service` is removed, the equivalent API being included into `Views Service`. Then, instead of:

```php
$template = Layout::make('default', $data, $template);
```
You should do:

```php
$template = View::makeLayout('default', $template);
```

Renamed the **Templates** to **Themes**

**Default** theme renamed to **Bootstrap** to avoid namespace conflicts due to the word ‘default’ being a reserved word in php

**AdminLTE** renamed to **AdminLite**

**public** renamed to **webroot** to give a clearer understanding of where to point your web root to.


## 3.75.15

Remove the loading of app/Config/* files, now them being managed by Config API. Introduce a Boot Stage speed improvement, loading the Config Files on demand.

## 3.75.14

Introduced a series of optimizations, including but not only: Service Providers also for Events and Routes into Application, Boot Stage optimizations and an improved handling of the `Config` Options stored into Database.

Notable, the Routes defined into `app/Routes.php` are added by default using the Controllers namespace `App\Controllers`, then will simplify a bit the Routes definition, doing instead of:

```php
Route::get('/',       'App\Controllers\Welcome@index');
Route::get('subpage', 'App\Controllers\Welcome@subPage');
```

this way:

```php
Route::get('/',       'Welcome@index');
Route::get('subpage', 'Welcome@subPage');
```

To note that this Routing default namespace could be adjusted into `App\Providers\RouteServiceProvider`.

The Config Options which are stored into Database are now handled via the new Model called `App\Models\Option`, which permit to improve their handling, but also give to end-user the ability to manage the Options as he wants and needs.

No APIs changes.

## 3.75.13

Series of small improvements, including moving of the Application's Storage folder to the root directory.

To note that the impact of this pull request in sites "to be updated" is very small, including also as only the removing of /app/Storage directory, while adding the /storage folder with its sub-directories.

## 3.75.12

Corrected image validation for users module and adjusted image profile path for adminLTE/backend.php

## 3.75.11

Improve the HTTP Exceptions handling.

## 3.75.10

Automatically setup the Encryption Key after Composer based installation.

## 3.75.7

Consolidate the Boot Stage and implement an Events driven Backend Menu.

SYSTEMDIR, now points to:

vendor/nova-framework/system/

Also, it is removed the SQL script scripts/nova_testing.sql which contains tables which are now not useful anymore.

As improvements, the Main Menu from the Backend, implemented by the shipped Modules, is moved to a Events driven system, making it dynamically.

Finally, this pull request remove the vendor existence check, considering that it is not useful anymore for Nova's target audience, which is supposed to be formed from users skilled enough to use ORM Models, the QueryBuilder and other advanced techniques.

## 3.75.5

Adjust the application to support renaming the Template Service as Layout. The Template Service was renamed as Layout, because what it handle really are the Layout files.

Also, this renaming leave room for a future Template Service for handling the whole Templates, similar with the Modules one.

## 3.75.4

Update app/Config.php to support the Config Cache

## 3.75.0

System has been moved to it's own repo and will be installed automatically into Vendor on install. All installs should be installed from the 3 branch.

Install with the termianl command:

```php
composer create-project nova-framework/framework foldername 3.* -s dev
```

Master branch for the dev team only.

## 3.74.0 changes:

All classes from the System directory now have a primary namespace of Nova. The major change is prefixing of System classes namespaces with Nova\\, then instead of:

```php
use Database\ORM\Model;
```

You should use:

```php
use Nova\Database\ORM\Model;
```

Also added is **translatable error pages** this means all error pages such as 400, 404 contain text that can be translated.

Added a new helper function **vendor_url()** and add the missing file app/Views/Error/500.php

Its usage is for Layouts and is simple as:

```php
vendor_url('dist/css/AdminLTE.min.css', 'almasaeed2010/adminlte')
```

**Improve the HTTP Exceptions handling**

Improved the HTTP Exceptions handling, simplifying and unifying the logic, making possible to expose the entire Error Pages generation to the end-user, them making possible their customizations as Template, Layout, etc.

As a collateral consequence, the Response::error() was removed, not being useful anymore.

As usage the App::abort():

```php
App::abort(404);
```

Same way for going i.e. Error 400

```php
App::abort(400);
```

## 3.73.3 changes:

site_url() no longer returns a trailing slash

## 3.73.3 changes:

The following helpers have been removed

* PHPMailer
* CSRF
* Form
* Hooks
* Request
* Pagantor
* Password
* Session
* Url


# Routing images / js / css files
From within Templates your css, js and images must be in a Assets folder to be routed correctly. This applies to Modules as well, to have a css file from a Module the css file would be placed inside **app/Modules/ModuleName/Assets/css/file.css.** Additionally there is an Assets folder in the root of nova this is for storing resources outside of templates that can still be routed from above the document root.

Routing CSS:

CSS files can be loaded by using Assets::css() and passing in an array of css paths.
site_url() is used to determine the website url. Place paths to the files relative to the root.
template_url() is used to load resources from the template. It accepts two params:
1 - path to the css file relative to the theme's root.
2 - the theme name to be used

```php
Assets::css([
    //load from vendor
    site_url('vendor/twbs/bootstrap/dist/css/bootstrap.min.css'),
    site_url('vendor/twbs/bootstrap/dist/css/bootstrap-theme.min.css'),
    //external
    'https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css',
    //load from template
    template_url('css/style.css', 'Default'),
]);
```

To load JS is the same process only this time its Assets::js

```php
Assets::js([
    //external
    'https://code.jquery.com/jquery-1.12.4.min.js',
    //vendor
    site_url('vendor/twbs/bootstrap/dist/js/bootstrap.min.js'),
]);
```

Loading images, since the images are above the document root they have to be served from Nova, this is done by either using template_url() for images in the theme or resource_url() for resources in the global assets folder or from a module.

template_url() accepts two params:
1 - path to the image file relative to the theme's root.
2 - the theme name to be used

```php
<img src='<?= template_url('images/nova.png', 'Default'); ?>' alt='logo'>
```

For images, js and css files located in /assets
resource_url() accepts two params:
1 - path to the resource
2 - optionally the name of the module

```php
<img src='<?= resource_url('images/nova.png', 'Default'); ?>' alt='logo'>
```

## Router

### Named Params

All params should use the format of {paramname} the value in the route should match the param ie $paramname

```php
Route::get('user/{id}', function($id){
  echo $id;
})
```

>**Note**  from version 3.73.0 unnamed params have been removed.

### Optional Parameters
New to 3 is allowing params to be optional.

For a param to be optional add ? to the end of the param followed by a where clause:

```php
Route::get('user/{id?}', function($id = null){
  echo $id;
})->where('slug', '(.*)');
```

### Route Pattern

As from **3.73.0** instead of doing:

```php
->where('slug', '(.*)’);
```

On every Route definition which need that, you can setup a pattern with this parameter name a (regex) pattern, and it will be applied to any parameter with this name, aka {slug}.
Logically, the patterns **should** be defined before the Routes definition.

```php
Route::pattern('slug', '(.*)’);
```


### Groups
Routes can now be placed in a group, this allows all routes within the group to inherit the group name.

```php
Router::group('admin', function() {
    Router::any('index', 'App\Controllers\Pages@index');
    Router::any('add', 'App\Controllers\Pages@add');
});
```
Is the equivalent to

```php
Router::any('admin/index', 'App\Controllers\Admin@index');
Router::any('admin/add', 'App\Controllers\Admin@add');
```

### Error Log
The error log is no longer a .html file but rather a log file located in app/Storage/Logs/error.log On a production server errors result in a user friendly error but when in development mode a rich error log will be printed to the screen.


### Namespace change
classes in app/Controller app/Model and app/Modules now have a namespace starting with App.

* App\Controllers
* App\Models
* App\Modules

That is only for classes within app, this is not needed for classes within system.

### Aliases for helpers within views
Helpers can now be used without using a use statement, app/Config/App.php contains an array of classes with their alias allowing classes to be used directly inside views.

Instead of doing:

```php
use Helpers\Session;

Session::set('item', 'value');

```
it can become:

```php
Session::set('item', 'value');
```

## New / Updated Helpers / Methods

### Url

**resource_url($path, $module = null)**

Returns the path the global assets folder or the assets folder of modules when passing the module name as a second param.

```php
$path = resource_url('somefile', 'FileManager');
```
will result into:

```php
/modules/file_manager/assets/
```
With no parameters will return:

```php
/assets/somefile
```
Which correspond with the generic Assets directory.

### Csrf
Updated **makeToken()** and **isTokenValid()** to require a name parameter, this allows using multiple times on a single page with unique names.

### Data
**create_key()** has been renamed to **createKey()**

### Inflector
Inflector is a doctrine class to allow transforming file paths used within the Url helper.

### Request
**get()** method added.

### ReservedWords
This class has a method getList which returns an array of reserved words include PHP 7's reserved words.
