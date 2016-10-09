Views are the visual side of the Nova, they are the HTML output of the pages. Views can be located directly inside the **app/Views** folder or in a sub folder, this helps with organising your views.

Views are called from controllers once called they act as included files outputting anything inside of them. They have access to any data passed to them.

### Using a view from a controller

To load a view in a controller call **return View::make()** it's important return is used without is the system Middleware will not be called and no view will be loaded. 

View::make accepts a path to the view relative to the **app/Views** folder. 
The second param is the data and the third param is the name of the module when loading module views.

```php
return View::make('Welcome/Welcome');
```

To load a view from a module pass the module name as a third parameter.

```php
return View::make('Posts/Post', ['posts' => $posts], 'Modulename');
```

Another way and one that is **Recommended** is to call **return $this->getView()**
This will automatically determine the path and view file to be loaded based on the controller and method used.

For instance:

```php
class Welcome extends Controller
{
    public function index()
    {
        return $this->getView()
    }
}
```

This snippet is from the Welcome controller, from this Nova know's to load **app/Views/Welcome/Index.php** this worked out automatically when using this approach a folder matching the controller and method should exist in the Views folder. 

For Modules view folders should be placed inside **app/Modules/ModuleName/Views/Controllername/Viewfile.php**

For example a module called Blog has a method called post would have a view path of **app/Modules/Blog/View/Blog/Post.php**



## Inside a view

Views are normal PHP files, they can contain PHP and HTML, as such any PHP logic can be used inside a view though it's recommended to use only simple logic inside a view anything more complex is better suited inside a controller.

An example of a view; looping through an array and outputting its contents:

```php
 <p>Contacts List</p>
 <?php 
 if ($contacts) {
    foreach ($contacts as $row) {
        echo $row.'<br />';
    }
 }
 ?>
```

To return a view and store its contents use View::fetch, fetch takes 3 params:
1. The view path relative to the view folder or module
2. The data being passed
3. Optional when loading a view from a module pass in the module name.

```php
$content = View::fetch('Page/Show', $data, 'Pages');

echo $content;
```
## Post Processing

a new method, called '**after**', which is automatically executed when the current Action return a value different of null or boolean.

This post processing ability can be very useful in the **RESTful** Controllers, for example doing:

```php
public function index()
{
    $data = array(
         'success' => true
    );

    return $data;
}

public function show($id)
{
    $data = array(
         'success' => true
    );

    return $data;
}

public function after($data)
{
    header('Content-Type: application/json');

    echo json_encode($data);
}
```

Also, this post-processing can be very useful when it is used a Layout style rendering, to not write again and again the same snippets; as in example:

```php
public function index()
{
    $data['title'] = 'welcomeText';
    $data['welcomeMessage'] = 'welcomeMessage';

    // Render the View and fetch the output in a data variable.
    $content = View::fetch('Welcome/Welcome', $data);

    return $content;
}

public function after($data)
{
    return $this->getView()->withData($data);
}
```

The returned value of the current Action is passed to post-processing method as a parameter.

Alternative **View/Layout** options:

### Basic Commands

While the actual **View\View** methods are static and they should call independently, the new API works with View instances, then we should build them. We have two methods of disposition, for standard Views and Templated one. A combined usage example is presented below:

```php
return View::make('Welcome/SubPage')
    ->shares('title', $title)
    ->with('data', $data);

// OR

$page = View::make('Welcome/SubPage')->with('data', $data);

return View::makeTemplate('default')
    ->shares('title', $title)
    ->withContent($page);
```

View Methods can be chained.
shares is a way to share a variable that is accessible to the view files, useful for settings the page title.
to pass date ->with() command is used. With accepts 2 params:
1. the variable name to set
2. the value

Another way to set the variable is to add the name to end of ->with for example to pass a variable called contacts: 

```php
->withContacts($data)
```

The data can be passed to a View instance in diverse ways. The following commands are equivalent:

```php
$page = View::make('Welcome/SubPage');
$page->with('info', $info);
$page->withInfo($info);
$page->info = $info;
$page['info'] = $info;
```

To note the variable name transformation by dynamic **withX** methods.

Also, the View instances can be nested. The following commands are equivalent:

```php
// Add a View instance to a View's data
$view = View::make('foo')->nest('footer', 'Partials/Footer');

// Equivalent functionality using the "with" method
$view = View::make('foo')->with('footer', View::make('Partials/Footer'));
```

To note that nesting assumes that the nested View instance is a Standard View, not a Template one.

There is also a new **shares()** method, similar with actual **share()** but working for instances.

Every Controller has now the ability to specify its Template and Layout, as following:

```php
class Welcome extends Controller
{
    protected $template = 'Admin';
    protected $layout = 'custom';

}
```

WHERE the '**default**' Layout is a simple composition of your **header/footer** files. For details, see: **app/Templates/Default/default.php**

### Advanced Usage

Rendering with partials (i.e. **header/footer**) from the standard Views location
If you need to work with partials, for example blocks, header and footer files located in **app/Views** directory, it is very simple to do that. You have just to compose your views as following:

The following examples use a very simple Template Layout file, called:

**app/Templates/Default/custom.php**

Rendering with a complete custom Template living on Views folder

```php
return Template::make('custom')
   ->shares('title', $title)
   ->with('header', View::make('Partials/Header'))
   ->with('content', View::make('Page/Index', $data))
   ->with('footer', View::make('Partials/Footer'));
```

OR

```php
return Template::make('custom')
   ->shares('title', $title)
   ->withHeader(View::make('Partials/Header'))
   ->withContent(View::make('Page/Index', $data))
   ->withFooter(View::make('Partials/Footer'));
```

OR

```php
return Template::make('custom')
   ->shares('title', $title)
   ->nest('header', 'Partials/Header')
   ->nest('content', 'Page/Index', $data)
   ->nest('footer', 'Partials/Footer');
```

OR

```php
return Template::make('custom')
   ->shares('title', $title)
   ->with('content', View::make('Partials/Layout')
   ->nest('content', 'Page/Index', $data));
```
Rendering in the Style, but using the new API

```php
return Template::make('custom')
   ->shares('title', $title)
   ->with('header', View::makeTemplate('header'))
   ->with('content', View::make('Page/Index', $data))
   ->with('footer', View::makeTemplate('footer'))
```
