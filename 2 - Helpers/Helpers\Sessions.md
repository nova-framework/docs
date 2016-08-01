The session is a static class, this means it can be used in any controller without needing to be instantiated, the class has an init method if session_start() has not been set then it starts it. This call is in place in (core/controller.php) so it can already be used with no setup required.

The advantages of using a session class is all sessions are prefixed using the constant setup in the root index.php file, this avoid sessions clashing with other applications on the same domain.

**Usage**

Setting a session, call Session then ::set pass in the session name followed by its value
```php
Session::set('username','Dave');
```

To retrive an existing session use the get method:
```php
Session::get('username');
```

Destroy a session  by calling:
```php
Session::destroy();
```

To look inside the sessions array, call the display method:
```php
print_r(Session::display());
```
