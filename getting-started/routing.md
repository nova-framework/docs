Routing lets you create your own URL paths, based on the path you can load a closure or a controller.

## Routing Set-up
Namespaces are included in all classes. A namespace is like a layer, adding a namespace to a class means there can be multiple classes with the same name as long as each class is in a different namespace.

To define a route, call the static name **Route::** followed by either a post or a get to match the HTTP action. Next, set the path to match and call a closure or a controller.

```php
Route::get('', 'closure or controller');
```

To respond to both get and post requests use a match.

A match takes 3 params
1. HTTP type ie post, get
2. the url patten to match against ie /users
3. a closure or controller path

```php
 Route::match(['get', 'post'], 'users'  function(){
 
 });
```

## Closures

A closure is a function without a name, they are useful when you only need simple logic for a route, to use a closure first call **Route::** then set the URL pattern you want to match against, followed by a function.

```php
Route::get('simple', function() { 
  //do something simple
});
```

Controllers and models can also be used in a closure by instantiating the root controller.

```php
$c = new App\Controllers\BaseController();
$m = new App\Models\Users(); 

$m->getUsers();
```

Having said that it's best to use a controller, if you need access to a model.

Closures are convenient but can soon become messy.

## Controllers

To call a route to a controller, instead of typing a function you can enter the controller name and specify what method of that class you wish to load. They are dictated by an **@** symbol.

For example, to have a controller called **Users** (in the root of the controllers folder) and to load a **usersList method**, you would use the following:

```php
Route::get('users', 'Users@usersList');
```

The above would call the users controller and the userList method when **/users** is located in the URL, via a get request.

Routes can respond to both GET and POST requests. All post routes require a CSRF filter to ensure the request has come from the application and it not an attempted attack.

This means the posted data should also contain a valid $csrfToken ie:

```php 
<form action='<?=site_url('blog');?>' method='post'>
<input type='hidden' name='csrfToken' value='<?=$csrfToken;?>'>
```

To use a post route:

```php
Route::post('blog', 'Blog@store'));
```

## Groups
Routes can be placed in a group, which allows all routes within the group to inherit the group name. The first param is an array of options at the least is requires a prefix. The second param is a closure where sub routes are placed:

```php
Route::group(['prefix' => 'blog'], function() {
    Route::get('/', 'Blog@Index');
    Route::get('add', 'Blog@Add');
});
```

Is the equivalent to:

```php
Route::any('blog', 'Blog@index');
Route::any('blog/add', 'Blog@add');
```

### Group Prefixes and Namespaces

The **Route::group()** can also accept an array as the first parameter and permit commands like:

```php
Router::group(['prefix' => 'admin', 'namespace' => 'Admin'], function() {
    Route::match('get',               'users',               'Users@index');
    Route::match('get',               'users/create',        'Users@create');
    Route::match('post',              'users',               'Users@store');
    Route::match('get',               'users/(id)',        'Users@show');
    Route::match('get',               'users/(id)/edit',   'Users@edit');
    Route::match(['put', 'patch'],    'users/(id)',        'Users@update');
    Route::match('delete',            'users/(id)',        'Users@destroy');
});
```

Where the prefix **admin** will turn the route **users/create** into **admin/users/create** and the namespace **Admin** will prepend onto **Users@create**, turning into **App\Controllers\Admin\Users@create**

### Route::resource()

The **Route::resource()** method introduces the ability to write the group of resourceful routes, with the following specifications:

|HTTP Method|Route|Controller Method|
|---|---|---|
|GET|/posts|index|
|GET|/posts/create|create|
|POST|/posts|store|
|GET|/posts/{id}|show|
|GET|/posts/{id}/edit|edit|
|PUT/PATCH|/posts/{id}|update|
|DELETE|/posts/{id}|destroy|

The previous code snippet can now be written as:

```php
Route::group(['prefix' => 'admin', 'namespace' => 'Admin'], function() {
    Route::resource('posts', 'Posts');
});
```

OR

```php
Route::resource('admin/posts', 'Admin\Posts');
```

## Named Parameters

Route parameters are always encased within `{}` braces and should consist of alphabetic characters. Route parameters may not contain a `( - )` character. Use an underscore `( _ )` instead.

Occasionally, you may need to specify a route parameter, but make the presence of that route parameter optional. You may do so by placing a `?` mark after the parameter name.

```php
Route::get('request/{id?}', 'Demos@request');
```

You may also use Regular Expression Route Constraints, the below constraint will only allow numerical values for the id, using regex.

```php
Route::get('request/{id}', 'Demos@request')->where('id', '[0-9]+');
```

If you have multiple parameters with constraints, you may pass an array to the `where()` method.

```php
Route::get('request/{id}/{name}', 'Demos@request')
    ->where([
        'id' => '[0-9]+', 
        'name' => '[a-z]+'
    ]);
```

> **Note:** the `where()` method is not available on Unnamed Parameters.

To use a wildcard route add a slug route passing to a catchall method adding a where slug to the end to send all requests to this route. 

> **Please Note** this route should come last, any routes following this route will not be executed.

```php
Route::any('{slug}', 'Demo@catchAll')->where('slug', '(.*)');
```

If you wanted to set a default value to a named parameter, you may also use the `defaults()` method:

```php
Route::get('request/{id}/{name?}', 'Demos@request')
    ->defaults([
        'id' => 0,
        'name' => NULL
    ]);
```
