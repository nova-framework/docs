## Route Filters
How do they work? Let's say that we want a CSRF filter. In the file **app/Filters.php** we define it as following:

```php
Route::filter('csrf', function($route, $request) {
    $session = $request->session();

    $ajaxRequest = $request->ajax();

    $token = $ajaxRequest ? $request->header('X-CSRF-Token') : $request->input('csrfToken');

    if ($session->token() == $token) {
        //
    }

    // The CSRF Token is invalid, respond with Error 400 (Bad Request)
    else if ($ajaxRequest) {
        return Response::make('Bad Request', 400);
    } else {
        App::abort(400, 'Bad Request');
    }
});
```

We'll see that a route filter definition have a name as first parameter and secondly, a callback which receive a **Routing\Route** instance, which is just current matched route, from where being available into callback information about HTTP method, URI, captured parameters, route callback, etc.

**ATTENTION: WHEN one of the Filters returns boolean FALSE, the Routing will generate a "404 Error" for the matched Route even it is a valid matched one.**

This is useful to "hide" parts of your website for non-authenticated users or to redirect to a custom "404 Error" page, for example.

Note that Route Filters are defined using **"Route::filter()"**

How to use this Filter? We use a new style of defining Routes:

```php
Router::post('contact', array('before' => 'csrf', 'uses' => 'Contact@store'));
```

WHERE the Route definition accepts an array as a second parameter and where the keys name is obvious. The key **before'** assign to the value and the key **uses** assign the associated callback for the route.

Running this route definition, the routing will be known to apply the filter with the name 'csrf' before the controller execution, then on CSRF fault, the filter's callback will be executed and we go very fast into a redirect.

It's possible to apply multiple Filters to a Route, using a string containing their name separated by character **'|' (pipe)**.

Usually, we will want to add another two route filters and there is a more complex example:

```php
Route::filter('csrf', function($route, $request) {
    $session = $request->session();

    $ajaxRequest = $request->ajax();

    $token = $ajaxRequest ? $request->header('X-CSRF-Token') : $request->input('csrfToken');

    if ($session->token() == $token) {
        //
    }

    // The CSRF Token is invalid, respond with Error 400 (Bad Request)
    else if ($ajaxRequest) {
        return Response::make('Bad Request', 400);
    } else {
        App::abort(400, 'Bad Request');
    }
});

Route::filter('guest', function($route, $request) {
    if (Auth::guest()) {
        //
    }

    // User is authenticated.
    else if (! $request->ajax()) {
        return Redirect::to('admin/dashboard');
    } else {
        return Response::make('Unauthorized Access', 403);
    }
});
```

And an example of their usage can be:

```php
Route::get('contact', array(
    'before' => 'guest',
    'uses' => 'Contact@index'
));

Route::get('login',  array(
    'before' => 'guest',
    'uses' => 'Auth@login'
));

Route::get('logout', array(
    'before' => 'auth',
    'uses' => 'Auth@logout'
));
```
This results in only the guest users can access the contact and login page, while only the authenticated users can access the logout action.

## Using a closure with a route filter

Instead of calling a controller pass a closure to the end of the route call:

```php
Route::get('whoami', array('before' => 'auth', 'uses' => function()
{
    $user = Auth::user();

    return Response::json($user);
}));
```
