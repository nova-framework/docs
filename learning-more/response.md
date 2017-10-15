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