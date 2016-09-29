The controller's middleware, represents a high-level processing API, executed by the requested controller, when it is instantiated, its requested method is known as being valid and callable, and working in the same flow as your wanted method.

The graph of The Controller Execution Flow is as follow:

**before() -> action() -> after()**

While very efficient and accurate, sometimes this design is not the best. For example, to instantiate a controller and start its execution flow, only to obtain a redirect from a CSRF fault, can be costly as resources and response speed.

Better is to have a solution for routing to handle the CSRF fault, before to even instantiate the requested controller, right after the route was identified; this was the used resources will be lower and response speed will be better.

Enter the route filters: a method to execute specified callbacks right after the correct route was identified and before starting the execution of associated controller.

## Route Filters
How do they work? Let's say that we want a CSRF filter. In the file **app/Filters.php** we define it as following:

```php
Route::filter('csrf', function($route) {
    if (! Csrf::isTokenValid()) {
        return Redirect::to('path');
    }
});
```

We'll see that a route filter definition have a name as first parameter and secondly, a callback which receive a **Routing\Route** instance, which is just current matched route, from where being available into callback information about HTTP method, URI, captured parameters, route callback, etc.

**ATTENTION: WHEN one of the Filters returns boolean FALSE, the Routing will generate a "404 Error" for the matched Route even it is a valid matched one.**

This is useful to "hide" parts of your website for non-authenticated users or to redirect to a custom "404 Error" page, for example.

Note that Route Filters are defined using **"Route::filter()"**

How to use this Filter? We use a new style of defining Routes:

```php
Router::post('contact', array(
    'filters' => 'csrf',
    'uses' => 'App\Controllers\Contact@store'
));
```

WHERE the Route definition accepts an array as a second parameter and where the keys name is obvious. The key **filters'** assign to the value of a **'|'** separated string of used route filters, and the key 'uses' assign the associated callback for the route.

Running this route definition, the routing will be known to apply the filter with the name 'csrf' before the controller execution, then on CSRF fault, the filter's callback will be executed and we go very fast into a redirect.

It's possible to apply multiple Filters to a Route, using a string containing their name separated by character **'|' (pipe)**.

Usually, we will want to add another two route filters and there is a more complex example:

```php
Route::filter('csrf', function($route) {
    if (($route->method() == 'POST') && ! Csrf::isTokenValid()) {
        return Redirect::to('path');
    }
});

Route::filter('auth', function($route) {
    if (Session::get('loggedIn') == false) {
        return Redirect::to('login');
    }
});

Route::filter('guest', function($route) {
    if (Session::get('loggedIn') != false) {
        return Redirect::to('path');
    }
});
```

And an example of their usage can be:

```php
Router::get('contact', array(
    'before' => 'guest|csrf',
    'uses' => 'App\Controllers\Contact@index'
));

Router::get('login',  array(
    'before' => 'guest|csrf',
    'uses' => 'App\Controllers\Auth@login'
));

Router::get('logout', array(
    'before' => 'auth',
    'uses' => 'App\Controllers\Auth@logout'
));
```
WHERE only the only guest users can access the contact and login page, with CSRF validation, while only the authenticated users can access the logout action.

The alternative usage of route filters registering is to use a class instead of callback, where the called method will receive the matched route instance as a parameter. For example:

```php
Route::filter('auth', 'App\Helpers\Filters\User@isLoggedIn');
Route::filter('guest', 'App\Helpers\Filters\User@isGuest');
```

### Improvements
An improved Method handling when the routes are registered and a new router command called **share()**, which permit to register multiple routes all pointing to the same controller. 

For example:

```php
Router::share(array(
    array('GET', '/'),
    array('POST', '/home')
), 'App\Controllers\Home@index');
```