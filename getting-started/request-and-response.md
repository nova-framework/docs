The Request class provides many methods for examining the HTTP request for your application and extends the `Symfony\Component\HttpFoundation\Request` class.

**Please note that both Input and Request do NOT sanitise your data, it is up to you to do that.**

#### Retrieving The Request URI

```php
$uri = Request::path();
```

#### Retrieving The Request Method

```php
$method = Request::method();

if (Request::isMethod('post'))
{
    //
}
```

#### Determine If The Request Path Matches A Pattern

```php
if (Request::is('admin/*'))
{
    //
}
```

#### Get The Request URL
```php
$url = Request::url();
```

#### Retrieve A Request URI Segment
```php
$segment = Request::segment(1);
```

#### Retrieving A Request Header
```php
$value = Request::header('Content-Type');
```

#### Retrieving Values From SERVER

```php
$value = Request::server('PATH_INFO');
```

#### Determine If The Request Is Over HTTPS
```php
if (Request::secure())
{
    //
}
```

#### Determine If The Request Is Using AJAX
```php
if (Request::ajax())
{
    //
}
```

#### Determine If The Request Has JSON Content Type
```php
if (Request::isJson())
{
    //
}
```

#### Determine If The Request Is Asking For JSON
```php
if (Request::wantsJson())
{
    //
}
```

#### Checking The Requested Response Format

The `Request::format` method will return the requested response format based on the HTTP Accept header:

```php
if (Request::format() == 'json')
{
    //
}
```

This improved **Response** API, able to simplify the Framework's Response management. It is now possible to do in a Controller Method:

after doing a use:

```php
use Response;
```

Commands:

```php
// Create a Response instance with string content
return Response::make(json_encode($user));

// Create a Response instance with a given status
return Response::make('Not Found', 404);

// Create a Response with some custom headers
return Response::make(json_encode($user), 200, array('header' => 'value'));

// Create a response instance with JSON
return Response::json($data, 200, array('header' => 'value'));

// Create a 404 response with data (will be directly obtained a shiny themed Error Page)
return Response::error('404', array('error' => 'Not Found'));
```

It is also possible to use those Response instances in the Route Filters, in the case when the given Response will be sent to the browser and the further process will be skipped.

The new Response API permits further simplifications on Routing.
