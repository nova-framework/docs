The middleware gives a way execute code before the controllers are executed, they run before executing the requested Method, and store the resulted data in a Views shared storage. From where its loaded and added for rendering execution. This style avoids multiple calling of those Hooks, as in for every rendering call.

In the **View\View** there is a Method, called "share", permitting to set data variables which are shared by any called rendering method. For example, it is possible to have:

```php
return View::share('title', 'Page Title');
```

Making the variable **'title'** available in any theme layout. 


Middleware allows a method to be called before the controller is executed for example a controller could have this construct:

```php
public function __construct()
{
    parent::__construct();

    //
    $this->beforeFilter('@adminUsersFilter');
}
```

This would call the method adminUsersFilter:

```php
/**
 * A Before Filter which permit the access to Administrators.
 */
public function adminUsersFilter(Route $route, Request $request)
{
    // Check the User Authorization - while using the Extended Auth Driver.
    if (! Auth::user()->hasRole('administrator')) {
        $status = __('You are not authorized to access this resource.');

        return Redirect::to('admin/dashboard')->withStatus($status, 'warning');
    }
}
```

Controller methods should always return to allow the middleware execution to complete.

## RESTful API

A typical setup for a RESTful controller is:

```php
public function index()
{
    $data = array(
         'success' => true
    );

    return $this->getResponse($data);
}

public function show($id)
{
    $data = array(
         'success' => true
    );

    return $this->getResponse($data);
}

protected function getResponse($data)
{
    return Response::json($data);
}
```

This method `getResponse()` could be the perfect place to add suplementary processing, for example an OAUTH signature.

> When doing multiple RESTful Controllers, It's possible to create App\Core\ApiController which give you this method by default. Also when creating a RESTful API, it is better to use directly `Nova\Routing\Controller` due to this executing faster than `App\Core\Controller`.

