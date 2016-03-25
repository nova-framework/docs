Nova Framework is a PHP 5.5+ MVC Framework. It's designed to be lightweight and modular, allowing developers to build better and easy to maintain code with PHP. 

To this end Nova does not come with lots of built in libraries / helpers or modules instead, it's left to the user to decide what they want to implement, this allows freedom to design and build how you see fit.

The base framework however does come with a range of helper classes. New classes can easily be added at any stage of development.

This has been tested with php 5.6 and php 7 please report any bugs.

<br>
## Routing images / js / css files

From within Templates your css/js and images must be in a Assets folder to be routed correctly.
This applies to Modules as well, to have a css file from a Module the css file would be placed inside nova/app/Modules/ModuleName/Assets/css/file.css.
Additionally there is an Assets folder in the root of nova this is for storing resources outside of templates that can still be routed from above the document root.

<br>
## Router

<br>
##Optional parameters 

New to 3.0 is allowing filters to be optional 

Filters written like (:any) are required to match the route but writing a filter as (/(:any)) makes it optional.

This route supplied with Nova has one filter that is required then a further 3 optional filters. Multiple filters should be inside the first parenthesis. 

````
Router::any('admin/(:any)(/(:any)(/(:any)(/(:any))))', 'App\Controllers\Demo@test');
````

<br>
##Groups

new to 3.0 is groups routes can now placed in a group, this allows all routes within the group to inherit the group name

````
Router::group('admin', function() {
    Router::any('add', 'App\Controllers\Demo@cool');
    Router::any('settings', 'App\Controllers\Demo@nice');
});
````

Is the equivalent to

````
Router::any('admin/add', 'App\Controllers\Admin@add');
Router::any('admin/settings', 'App\Controllers\Admin@settings');
````

<br>
## Error Log

The error log is no longer a .html file but rather a log file. On a production server it should be outside the document root, in order to see the any errors there are a few options:

* Open app/logs/error.log
* OR open system/Core/Logger.php set $display to true to print errors to the screen
* set $emailError to true and setup the siteEmail const in app/Config.php this relies on an email server (not provided by the framework)

<br>
##Namespace change

classes in app/Controller app/Model and app/Modules now have a namespace starting with App:

* App\Controllers
* App\Models
* App\Modules

That is only for classes within app, this is not needed for classes within system.

<br>
## Aliases for helpers within views

Helpers can now be used without using a use statement, inside system/Core/Alias contains an array of helpers with their alias allowing helpers to be used directly.

Instead of doing:

````
use Helpers\Session;

Session::set('item', 'value');
````

it can become:

````
Session::set('item', 'value');
````

<br>
# New / Updated Helpers/Methods

<br>
## Url

<br>
**resourcePath()**
The basic idea is to provide a lowercase version of the resource path for the resources located in Modules and the base assets folder. Then the following call:

````
$path = Url::resourcePath('FileManager');
````

will result into:

````
/modules/file_manager/assets/
````

With no parameters will return:

````
/assets/
````

Which correspond with the generic Assets directory.

<br>
**detectUri()**

Returns the current url.

<br>
**templatePath()** and **relativeTemplatePath()**

Now accepts 2 parameters:
1. $custom - default to TEMPLATE which is the template being used
2. $folder - folder to return from within the template defaults to Assets


<br>
## Assets

The assets helper loads css and js files both methods have the following parameters:

- $files - a single file or array of file paths
- $cache - cache the output default to false, a minified file will be generated in the theme css/js folder.
- $refresh - update the cache default to false
- $cachedMins - time in seconds to keep the cache for defaults to 14400

<br>
## Csrf

Updated makeToken() and isTokenValid() to require a name parameter, this allows using multiple times on a single page with unique names.

<br>
## Data

create_key() has been renamed to createKey()

<br>
## Form

Form::open by default created a hidden input with a token to be used for cross site request forgery checks (CSRF) if a name is passed to Form::open it's used as part of the token name.

<br>
## Inflector

Inflector is a doctrine class to allow transforming file paths used within the Url helper.

<br>
## JsMin

Minifies supplied js code

<br>
## Request

get() method added.

<br>
## ReservedWords

This class has a method getList which returns an array of reserved words include PHP 7's reserved words.

<br>
## Response

This helper is primary used for routing, see the Response page for more details.

