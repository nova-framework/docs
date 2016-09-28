Routing lets you create your own URL paths, based on the path you can load a closure or a controller.

## Routing Set-up
Namespaces are included in all classes now. A namespace is like a layer, adding a namespace to a class means there can be multiple classes with the same name as long as each class is in a different namespace.

With routes the namespace is **Routing\Router::** followed by the method call, typing out the namespace every time is long winded, thankfully short cuts can be created by creating an alias:

````php
use Routing\Router;
```

By using the use keyword **Routing\Router**, it can be referenced as **Router**.

To define a route, call the static name **Router::** followed by either a post or a get (**'any'** can also be used to match both **post** and **get** requests) to match the HTTP action. Next, set the path to match and call a closure or a controller.

````php
Router::any('', 'closure or controller');
```

## Closures

A closure is a function without a name, they are useful when you only need simple logic for a route, to use a closure first call **Router::** then set the URL pattern you want to match against, followed by a function.

````php
Router::get('simple', function() { 
  //do something simple
});
```

[[Controllers]] and [[Models]] can also be used in a closure by instantiating the root controller.

````php
$c = new \App\Core\Controller();
$m = new \App\Models\Users(); 

$m->getUsers();
```

Having said that it's best to use a controller, if you need access to a model.

Closures are convenient but can soon become messy.

## Controllers
To call a route to a controller, instead of typing a function you can enter a string. In the string type the namespace of the controller (**App/Controllers** if located in the root of the controllers folder) then the controller name. Finally, specify what method of that class you wish to load. They are dictated by an **'@'** symbol.

For example, to have a controller called **Users** (in the root of the controllers folder) and to load a **usersList method**, you would use the following:

````php
Router::get('users', 'App\Controllers\Users@usersList');
```

The above would call the Users controller and the userList method when **/users** is located in the URL, via a get request.

Routes can respond to both GET and POST requests. 

To use a post route:

````php
Router::post('blogsave', 'App\Controllers\Blog@savePost');
```

To respond to either a post or get request, use any:

````php
Router::any('blogsave', 'App\Controllers\Blog@savePost');
```

## Groups
Group routes are new to 3.0. Routes can now be placed in a group, which allows all routes within the group to inherit the group name.

````php
Router::group('admin', function() {
    Router::any('add', 'App\Controllers\Demo@cool');
    Router::any('settings', 'App\Controllers\Demo@nice');
});
```

Is the equivalent to

````php
Router::any('admin/add', 'App\Controllers\Admin@add');
Router::any('admin/settings', 'App\Controllers\Admin@settings');
```

### Group Prefixes and Namespaces

The **Router::group()** can also accept an array as the first parameter and permit commands like:
````php
Router::group(['prefix' => 'admin', 'namespace' => 'App\Controllers\Admin'], function() {
    Router::match('get',            'users',             'Users@index');
    Router::match('get',            'users/create',      'Users@create');
    Router::match('post',           'users',             'Users@store');
    Router::match('get',            'users/(:any)',      'Users@show');
    Router::match('get',            'users/(:any)/edit', 'Users@edit');
    Router::match(['put', 'patch'], 'users/(:any)',      'Users@update');
    Router::match('delete',         'users/(:any)',      'Users@destroy');
});
```
Where the prefix **admin** will turn the route **users/create** into **admin/users/create** and the namespace **App\Controllers\Admin** will prepend onto **Users@create**, turning into **App\Controllers\Admin\Users@create**

### Router::resource()

The **Router::resource()** method introduces the ability to write the group of resourceful routes, with the following specifications:

|HTTP Method|Route|Controller Method|
|---|---|---|
|GET|/photo|index|
|GET|/photo/create|create|
|POST|/photo|store|
|GET|/photo/(:any)|show|
|GET|/photo/(:any)/edit|edit|
|PUT/PATCH|/photo/(:any)|update|
|DELETE|/photo/(:any)|destroy|

The previous code snippet can now be written as:

````php
Router::group(['prefix' => 'admin', 'namespace' => 'App\Controllers\Admin'], function() {
    Router::resource('users', 'Users');
    Router::resource('categories', 'Categories');
    Router::resource('articles', 'Articles');
});
```
OR
````php
Router::resource('admin/users', 'App\Controllers\Admin\Users');
Router::resource('admin/categories', 'App\Controllers\Admin\Categories');
Router::resource('admin/articles', 'App\Controllers\Admin\Articles');
```

## Implicit Controllers

An Implicit Controller allows you to easily define a single route to handle every action in a controller. 

First, define the route using the Route::controller method:
```php
Route::controller('users', 'App\Controllers\Users');
```

The `Route::controller()` method accepts two arguments. The first is the base URI the controller handles, while the second is the class name of the Controller. Next, just add methods to your Controller, prefixed with the HTTP verb they respond to: (get / post / any) 

```php
class App\Controllers;

use Core\Controller;

class Users extends Controller 
{
    public function getIndex()
    {
        //
    }

    public function postProfile()
    {
        //
    }

    public function anyLogin()
    {
        //
    }

}
```
The index methods will respond to the root URI handled by the Controller, which, in this case, is users.

If your controller action contains multiple words, you may access the action using hyphan - syntax in the URI. For example, the following controller action on our **App\Controllers\Users** would respond to the users/admin-profile URI:

```php
public function getAdminProfile() {}
```

Methods can take up to 7 params, if more are needed create a specific route in Routes.php

## Routing Filters
Routes can also use filters to dynamically pass values to the controller / closure, there are 3 filters:

1. **(:any) any** - can use characters or numbers
2. **(:num) num** - can only use numbers
3. **(:all) all** - will accept everything including any slash paths

To use a filter place the filter inside parenthesis and use a colon inside route path.

````php
Route::get('blog/(:any)', 'App\Controllers\Blog@post');
```

Would get past to **app/Controllers/Blog.php** anything after **blog/** will be passed to post method.

````php
public function post($slug)
{
    // Some code ...
}
```

### Optional Parameters
New to 3.0 is allowing filters to be optional

Filters which are written like **(:any)** are required to match the route but writing a filter as **(/(:any))** makes it optional.

This route supplied with Nova has one filter that is required then a further 3 optional filters. Multiple filters should be inside the first parenthesis.

````php
Route::any('admin/(:any)(/(:any)(/(:any)(/(:any))))', 'App\Controllers\Demo@test');
```

## Named Parameters

Alternatively, instead of using the Route Filters above, you may also use **Named Parameters**. 

Route parameters are always encased within `{}` braces and should consist of alphabetic characters. Route parameters may not contain a `( - )` character. Use an underscore `( _ )` instead.

Occasionally, you may need to specify a route parameter, but make the presence of that route parameter optional. You may do so by placing a `?` mark after the parameter name.

````php
Route::get('request/{param1}/{param2?}', 'Demos@request');
```

You may also use Regular Expression Route Constraints, the below contraint will only allow numerical values for the id, using regex.

````php
Route::get('request/{id}', 'Demos@request')
    ->where('id', '[0-9]+');
```

If you have multiple parameters with contraints, you may pass an array to the `where()` method.

````php
Route::get('request/{id}/{name}', 'Demos@request')
    ->where([
        'id' => '[0-9]+', 
        'name' => '[a-z]+'
    ]);
```

> **Please Note:** the `where()` method is not available on Unnamed Parameters.

If you wanted to set a default value to a named parameter, you may also use the `defaults()` method:

````php
Route::get('request/{id}/{name?}', 'Demos@request')
    ->defaults([
        'id' => 0,
        'name' => NULL
    ]);
```

> **Please Note:** the `defaults()` method is not available on Unnamed Parameters.
