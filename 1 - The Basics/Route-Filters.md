The Controller's [[Middleware]], represents a High-Level processing API, executed by the requested Controller, when it is instantiated, its requested Method is known as being valid and callable, and working in the same flow as your wanted Method.

The graph of The Controller Execution Flow is as follow:

**before() -> action() -> after()**

While very efficient and accurate, sometimes this design is not the best. For example, to instantiate a Controller and start its Execution Flow, only to obtain a redirect from a CSRF fault, can be costly as resources and response speed.

Better is to have a solution for Routing to handle the CSRF fault, before to even instantiate the requested Controller, right after the Route was identified; this was the used resources will be lower and response speed will be better.

Enter the Route Filters: a method to execute specified callbacks right after the correct Route was identified and before starting the execution of associated Controller.

## Route Filters
How do they work? Let's say that we want a CSRF filter. In the (new) file **app/Filters.php** we define it as following:

````php
Route::filter('csrf', function($route) {
    if (! Csrf::isTokenValid()) {
        return Redirect::to('');
    }
});
```

We'll see that a Route Filter definition have a name as first parameter and secondly, a callback which receive a **Core\Route** instance, which is just current matched Route, from where being available into callback information about HTTP method, URI, captured parameters, Route callback, etc.

**ATTENTION: WHEN one of the Filters returns boolean FALSE, the Routing will generate a "404 Error" for the matched Route even it is a valid matched one.**

This is useful to "hide" parts of your website for non-authenticated users or to redirect to a custom "404 Error" page, for example.

Note that Route Filters are defined using **"Route::filter()"**

How to use this Filter? We use a new style of defining Routes:

````php
Router::post('contact', array(
    'filters' => 'csrf',
    'uses' => 'App\Controllers\Contact@store'
));
```

WHERE the Route definition accepts an array as a second parameter and where the keys name is obvious. The key **filters'** assign to the value of a **'|'** separated string of used Route Filters, and the key 'uses' assign the associated Callback for the Route.

Running this Route definition, the Routing will be known to apply the Filter with the name 'csrf' before the Controller execution, then on CSRF fault, the Filter's callback will be executed and we go very fast into a redirect.

It's possible to apply multiple Filters to a Route, using a string containing their name separated by character **'|' (pipe)**.

Usually, we will want to add another two Route Filters and there is a more complex example:

````php
Route::filter('csrf', function($route) {
    if (($route->method() == 'POST') && ! Csrf::isTokenValid()) {
        return Redirect::to('');
    }
});

Route::filter('auth', function($route) {
    if (Session::get('loggedIn') == false) {
        return Redirect::to('login');
    }
});

Route::filter('guest', function($route) {
    if (Session::get('loggedIn') != false) {
        return Redirect::to('');
    }
});
```

And an example of their usage can be:

````php
Router::any('contact', array(
    'filters' => 'guest|csrf',
    'uses' => 'App\Controllers\Contact@index'
));

Router::any('login',  array(
    'filters' => 'guest|csrf',
    'uses' => 'App\Controllers\Auth@login'
));

Router::get('logout', array(
    'filters' => 'auth',
    'uses' => 'App\Controllers\Auth@logout'
));
```
WHERE only the only Guest Users can access the Contact and Login page, with CSRF validation, while only the Authenticated Users can access the Logout action.

The alternative usage of Route Filters registering is to use a Class instead of callback, where the called method will receive the matched Route instance as a parameter. For example:

````php
Route::filter('auth', 'App\Helpers\Filters\User@isLoggedIn');
Route::filter('guest', 'App\Helpers\Filters\User@isGuest');
```

### Improvements
An improved Method handling when the Routes are registered and a new Router command called **share()**, which permit to register multiple Routes all pointing to the same Controller. 

For example:

````php
Router::share(array(
    array('GET', '/'),
    array('POST', '/home')
), 'App\Controllers\Home@index');
```