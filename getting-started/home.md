![Nova Framework](http://novaframework.com/app/templates/publicthemes/nova/images/nova.png)

# Overview
Nova Framework is a PHP 5.6+ MVC Framework. It's designed to be lightweight and modular, allowing developers to build better and easy to maintain code with PHP.

To this end Nova does not come with lots of built in libraries / helpers or modules instead, it's left to the user to decide what they want to implement, this allows freedom to design and build how you see fit. Having said that there are a limited number provided.

This has been tested with php 5.6 and php 7 please report any bugs.

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
