# Routing 

Routing lets you create your own url paths, based on the path you can load a closure or a controller.

**Routing Setup**

Namespaces are included into all classes now, a namespace is like a layer. Adding a namespace to a class means there can be multiple classes with the same name as long as each is in a different namespace.

With routes the namespace is Core\Router:: followed by the method call, typing out the namespace every time is long winded, thankfully shortcuts can be created by creating an alias:

````
use Core\Router;
````

By using the use keyword Core\Router can be references as Router.

To define a route call the static name Router:: followed by either a post or get ('any' can also be used to match both post and get requests) to match the HTTP action. Next set the path to match and what do, call a closure or a controller.

````
Router::any('', 'closure or controller');
````

**Closures**

A closure is a function without a name, they are useful when you only need simple logic for a route, to use a closure first call Router:: then set the url pattern you want to match against followed by a function

````
Router::get('simple', function() {
  //do something simple
});
````

Controllers and model can be used in a closure by instantiating the root controller

````
$c = new Core\Controller();
$m = new Models\Users();

$m->getUsers();
````

Having said that it's better to use a controller once you need access to a model.

Closures are convenient but can soon become messy.

**Controllers**

To call a route to a controller instead of typing a function, instead enter a string. In the string type the namespace of the controller (Controller if located in the root of the controllers folder) then the controller name, finally specify what method of that class to load. They are dictated by a @ symbol.

To have a controller called users (in the root of the controllers folder) and load a usersList function:

````
Router::get('users','Controllers\Users@usersList');
````

The above would call the Users controllers and the userList method when /users is in the url via a get request.

Routes can respond to both GET and POST requests.

To use a post route:

````
Router::post('blogsave', 'Controllers\Blog@savePost');
````

To respond to either a post or get request use any:

````
Router::any('blogsave', 'Controllers\Blog@savePost');
````

**Routing Filters**

Routes can use filters to dynamically pass values to the controller / closure, there are 3 filters:

1.  (:any) any - can use characters or numbers
2.  (:num) num - can only use numbers
3.  (:all) all - will accept everything including any slash paths

To use a filter place the filter inside parenthesis and use a colon inside route path

````
Router::get('blog/(:any)', 'Controllers\Blog@post');
````

Would get past to app/Controllers/Blog.php anything after blog/ will be passed to post method.

````
public function post($slug)
{
````

If no route is defined, you can call a custom callback, like:

````
Router::error('Core\Error@index');
````

Finally to run the routes:

````
Router::dispatch();
````

## Full Example

````
use Core\Router;

//define routes
Router::get('', 'Controllers\Welcome@index');

//call a controller in called users inside a admin folder inside the controllers folder
Router::('admin/users','Controllers\Admin\Users@list');

//if no route found
Router::error('Error@index');

//execute matched routes
Router::dispatch();
````

##Legacy Calls

To enable legacy calls change the $fallback to true, default is false (off)

```` 
//turn on old style routing
Router::$fallback = true;
````

To call controllers and methods by their name and not a custom route can be done by calling the controller file name followed by the method. Any params are passed as keys to an array so only one param is used.

For instance to call the welcome controller and a method called custom enter the url http://example.com/welcome/custom

To load the index method: http://example.com/welcome the /index is not needed, it would be called automatically.

