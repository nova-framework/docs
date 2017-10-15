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

#### Retrieving Values From $_SERVER
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