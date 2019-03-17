The middleware gives a way execute code before the controllers are executed, they run before executing the requested method, and store the resulted data in a Views shared storage. From where its loaded and added for rendering execution. This style avoids multiple calling of those Hooks, as in for every rendering call.

Middleware allows a method to be called before the controller is executed for example a controller could have this:

```php
public function __construct()
{
    $this->middleware('authenticated');
}
```