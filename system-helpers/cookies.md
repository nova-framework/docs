The Cookie helper has the following methods:

````php
Cookie::exists($key);
```
Returns true or false

````php
Cookie::set($key, $value, $expiry = self::FOURYEARS, $path = "/", $domain = false);
```

* **$key** - the name of the cookie

* **$value** - the value the cookie holds

* **$expiry** - time to hold the cookie

* **$path** - the path the cookie accepts

* **domain** - the domain to be used defaults to false

````php
Cookie::get($key, $default = '')
```
Get the key from a cookie

````php
Cookie::display()
```
Returns the $_COOKIE

````php
Cookie::destroy($key, $path = "/", $domain = "")
```
Remove/Delete cookie match the id of $key