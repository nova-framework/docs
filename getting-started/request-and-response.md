The Request class provides many methods for examining the HTTP request for your application and extends the `Symfony\Component\HttpFoundation\Request` class.

**Please note that Request do NOT sanitise your data, it is up to you to do that.**

#### Retrieving The Request URI

```php
use use Nova\Http\Request;

$request = new Request();

$request->path();
```

#### Retrieving The Request Method

```php
$method = $request->method();

if ($request->isMethod('post'))
{
    //
}
```

#### Determine If The Request Path Matches A Pattern

```php
if ($request->is('admin/*'))
{
    //
}
```

#### Get The Request URL
```php
$url = $request->url();
```

#### Retrieve A Request URI Segment
```php
$segment = $request->segment(1);
```

#### Retrieving A Request Header
```php
$value = $request->header('Content-Type');
```

#### Retrieving Values From SERVER

```php
$value = $request->server('PATH_INFO');
```

#### Determine If The Request Is Over HTTPS
```php
if ($request->secure())
{
    //
}
```

#### Determine If The Request Is Using AJAX
```php
if ($request->ajax())
{
    //
}
```

#### Determine If The Request Has JSON Content Type
```php
if ($request->isJson())
{
    //
}
```

#### Determine If The Request Is Asking For JSON
```php
if ($request->wantsJson())
{
    //
}
```

#### Checking The Requested Response Format

The `Request::format` method will return the requested response format based on the HTTP Accept header:

```php
if ($request->format() == 'json')
{
    //
}
```

This improved **Response** API, able to simplify the Framework's Response management. It is now possible to do in a Controller Method:

after doing a use:

```php
use Nova\Support\Facades\Response;
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
```
