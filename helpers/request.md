#**From v3.73.3 this has been removed**

The **Helpers\Request** class is used for detecting the type of request and retrieving the request.

### getMethod()

```php
Request::getMethod()
```
Returns either GET or POST, depending if a $_GET request or a $_POST request has happened.

### getIpAddress()
```php
Request::getIpAddress()
```
Returns the client's IP Address.

### post()
```php
Request::post($key)
```
Returns the post key.

### get()
```php
Request::get($key)
```
Returns the get key.

### server()
```php
Request::server($key)
```
Returns the server key.

### headers()
```php
Request::headers($key)
```
Returns the HTTP Request Headers key.

### files()
```php
Request::files($key)
```
Returns the file key.

### put()
```php
Request::put($key)
```
Returns the put key.

### isAjax()
```php
Request::isAjax()
```
Detect if the request is a ajax request.

### isPost()
```php
Request::isPost()
```
Detect if the request is a post request.

### isGet()
```php
Request::isGet()
```
Detect if the request is a get request.

### isHead()
```php
Request::isHead()
```
Detect if the request is a head request.

### isPut()
```php
Request::isPut()
```
Detect if the request is a put request.

### isDelete()
```php
Request::isDelete()
```
Detect if the request is a delete request.

### isOptions()
```php
Request::isOptions()
```
Detect if the request is an options request.
