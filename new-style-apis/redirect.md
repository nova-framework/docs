To use this Class, add the following to the top of the file.

```php
use Redirect;
```

**Redirect::to($path, $status = 302, $headers = array(), $secure = null)**

Creates a new redirect response to the given path.

```php
// Create a Redirect Response to a location within the application
return Redirect::to('user/profile');

// Create a Redirect Response with a 301 status code
return Redirect::to('user/profile', 301);

// Create a Redirect Response and flash to the Session
return Redirect::to('profile')->with('message', 'Welcome Back!');
```

**Redirect::home($status = 302)**

Creates a new redirect response to the "home" route.\

```php
return Redirect::home();
```

**Redirect::back($status = 302, $headers = array())**

Create a new redirect response to the previous location.

```php
return Redirect::back(302);
```

**Redirect::refresh($status = 302, $headers = array())**

Create a new redirect response to the current URI.

```php
return Redirect::refresh(302);
```

**Redirect::guest($path, $status = 302, $headers = array(), $secure = null)**

Create a new redirect response, while putting the current URL in the session.

```php
return Redirect::guest('user/profile');
```

**Redirect::away($path, $status = 302, $headers = array())**

Create a new redirect response to an external URL (no validation).

```php
return Redirect::away('http://www.google.com');
```

**Redirect::secure($path, $status = 302, $headers = array())**

Create a new redirect response to the given HTTPS path.

```php
return Redirect::secure('user/profile');
```