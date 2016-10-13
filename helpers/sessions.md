#**From v3.73.3 this has been removed**

The session is a static class, this means it can be used in any controller without needing to be instantiated, the class has an init method if session_start() has not been set then it starts it. 

The advantages of using a session class is all sessions are prefixed using the constant setup in the root index.php file, this avoid sessions clashing with other applications on the same domain.

### Usage

Setting a session, call Session then ::set pass in the session name followed by its value

```php
Session::set('username', 'Dave');
```
To retrieve an existing session use the get method:

```php
Session::get('username');
```
Pull an existing session key and remove it, use the pull method:

```php
Session::pull('username');
```
Use id to return the session id.

```php
Session::id();
```
Destroy a session key by calling:

```php
Session::destroy('mykey');
```

To look inside the sessions array, call the display method:

```php
print_r(Session::display());
```
