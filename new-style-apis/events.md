The idea behind events is the ability to send data, as parameters, to listeners and call them when an event happens. The listeners could be a closure or a static class method.

To note that if the **Events\Dispatcher** does not instantiate a listener's class, then the called method must be static.

Its usage is simple; first, we have to register a listener, adding:

```php
// Add a Listener to the Event 'test'.
Event::listen('test', 'App\Events\Test@handle');
```

The handle:

```php
namespace App\Events;

class Test
{
    public static function handle($message)
    {
        echo $message;
    }
}
```

Then, when the payload is needed, we would fire the Event:

```php
// Prepare the Event payload.
$payload = array(
    'Hello, this is an Event!',
);

// Fire the Event 'test'.
Event::fire('test', $payload);
```

## Queued Events

The Queued Events is a method to add a number of Events to a Queue, then firing them together, flushing the Queue. E.g

```php
// Queue the Events
Event::queue('test', array('This is the First Event'));
Event::queue('test', array('This is the Second Event'));
Event::queue('test', array('This is the Third Event'));

// Flush the Queue, firing all defined Events
Event::flush('test');
```

### until() Method

The method **Events\Dispatcher@until** is about not firing an Event until the first Listener returns a valid and non-null response. 

Its usage is simple:

```php
$response = Event::until('testing', $payload);

if($response !== null) {
    // Do something with $response
}
```

### Ordering Events

Events can be ordered when using Event::listen by passing a third param of priority. It defaults to 0 the idea being the higher the number the higher priority is has.

```php
Event::listen('modules', function(){
	echo '<li><a href="'.site_url('admin/sidebars').'"><i class="fa fa-cubes"></i> Sidebars</a></li>';
}, 2);

Event::listen('modules', function(){
	echo '<li><a href="'.site_url('admin/widgets').'"><i class="fa fa-cubes"></i> Widgets</a></li>';
}, 1);
```

This will result in Sidebars coming before Widgets due to the higher priority number.