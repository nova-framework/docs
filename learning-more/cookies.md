All cookies created by the Nova framework are encrypted and signed with an authentication code, meaning they will be considered invalid if they have been changed by the client.

#### Retrieving A Cookie Value

```php
$value = Cookie::get('name');
```

#### Attaching A New Cookie To A Response

```php
$response = Response::make('Hello World');

$response->withCookie(Cookie::make('name', 'value', $minutes));
```

#### Queueing A Cookie For The Next Response

If you would like to set a cookie before a response has been created, use the `Cookie::queue()` method. The cookie will automatically be attached to the final response from your application.

```php
Cookie::queue($name, $value, $minutes);
```

#### Creating A Cookie That Lasts Forever

```php
$cookie = Cookie::forever('name', 'value');
```

#### Forgetting A Cookie
```php
Cookie::forget('name');
```