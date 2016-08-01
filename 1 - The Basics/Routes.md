A new feature in v2 is routing. Routing lets you create your own url paths, based on the path you can load a closure or a controller!

Routing is built into the v2, nothing needs to be called in order to use a route, you only need to create your route in index.php

**Routing Setup**
To define a route call the static name Route:: followed by either a post or get (any can also be used to match both post and get requests) to match the HTTP action. Next set the path to match and what do, call a closure or a controller.

```php
Router::any('', 'closure or controller');
```

**Closures**

A closure is a function without a name, they are useful when you only need simple logic for a route, to use a closure first call Route:: then set the url pattern you want to match against followed by a function

```php
Router::get('simple', function(){
  //do something simple
});
```

Controllers and model can be used in a controller by instantiating the root controller

```php
$c = new Controller();
$users = $c->loadModel('users_model');

$users->get_users();
```

Having said that it's better to use a controller once you need access to a model.

Closures are convenient but can soon become messy.

**Controllers**

To call a route to a controller instead of typing a function instead enter a string. In the string type the path to the controllers then the controller name, finally specify what method of that class to load. They are dictated by a @ symbol.

Say I have a controller called users (in the root of the controllers folder) and I want to load a userslist function:
```php
Route::get('users','users@userslist');
```

The above would call the users controllers and the userlist method when /users is in the url via a get request.

Routes can respond to both GET and POST requests
To use a post route:
```php
Router::post('blogsave', 'blog@savepost');
```

To respond to either a post or get request use any:
```php
Router::any('blogsave', 'blog@savepost');
```

**Routing Filters**

Routes can use filters to dynamically pass values to the controller / closures, their are 3 filters:

- any - can use characters or numbers
- num - can only use numbers
- all - will accept everything including any slash paths

To use a filter place the filter inside parenthesis and use a colon inside route path

```php
Router::get('blog/(:any)', 'blog@post');
```

Would get past to app/controllers/blog.php anything after blog/ will be passed to post method.

```php
public function post($slug){
```

If there is no route defined, you can call a custom callback, like:
```php
Router::error('error@index');
```

Finally to run the routes:
```php
Router::dispatch();
```

## Full Example
```php
//define routes
Router::get('', 'welcome@index');

//if no route found
Router::error('error@index');

//execute matched routes
Router::dispatch();
```
