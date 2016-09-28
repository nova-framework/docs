Views are the visual side of the Nova, they are the HTML output of the pages. Views can be located directly inside the views folder or in a sub folder, this helps with organising your views.

Views are called from controllers once called they act as included files outputting anything inside of them. They have access to any data passed to them.

The render method is used to include a view file, the method expects the path to the view. Optionally an array can be passed.

The renderTemplate is almost the same except its use is for including templates, useful for including header and footer files for your application's design. The template defined inside the **TEMPLATE** constant is used by default but passing the third parameter to renderTemplate containing a string can be used to use a different template folder.

For example, calling an email template can be done like this:

````php
View::renderTemplate('header', $data, 'email');
```

The template folder used is dictated by the template set in the **app/Config.php** file via a constant.

### Using a view from a controller

A view can be set inside a method, an array can optionally be created and passed to both the render and renderTemplate methods, this is useful for setting the page title and letting a header template use it.

````php
 $data['title'] = 'Welcome';

 View::renderTemplate('header', $data);
 View::render('Welcome/Welcome', $data);
 View::renderTemplate('footer', $data);
```

## Inside a view

Views are normal PHP files, they can contain PHP and HTML, as such any PHP logic can be used inside a view though it's recommended to use only simple logic inside a view anything more complex is better suited inside a controller.

An example of a view; looping through an array and outputting its contents:

````php
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

````php
$content = View::fetch('Page/Show', $data, 'Pages');

echo $content;
```

OR

````php
$data['content'] = View::fetch('Welcome/SubPage', $data);

View::renderTemplate('default', $data);
```

a new method, called '**after**', which is automatically executed when the current Action return a value different of null or boolean.

This post processing ability can be very useful in the **RESTful** Controllers, for example doing:

````php
public function index()
{
    $data = array(
         'success' = true;
         ...
    );

    return $data;
}

public function show($id)
{
    $data = array(
         'success' = true;
         ...
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

````php
public function index()
{
    $data['title'] = $this->trans('welcomeText');
    $data['welcomeMessage'] = $this->trans('welcomeMessage');

    // Render the View and fetch the output in a data variable.
    $data['content'] = View::fetch('Welcome/Welcome', $data);

    return $data;
}

public function subPage()
{
    $data['title'] = $this->trans('subpageText');
    $data['welcomeMessage'] = $this->trans('subpageMessage');

    // Render the View and fetch the output in a data variable.
    $data['content'] = View::fetch('Welcome/SubPage', $data);

    return $data;
}

public function after($data)
{
    View::renderTemplate('default', $data);
}
```

The returned value of the current Action is passed to post-processing method as a parameter.

Alternative **View/Layout** options:

### Basic Commands

While the actual **Core\View** methods are static and they should call independently, the new API works with View instances, then we should build them. We have two methods of disposition, for standard Views and Templated one. A combined usage example is presented below:

````php
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

````php
->withContacts($data)
````

The data can be passed to a View instance in diverse ways. The following commands are equivalent:

````php
$page = View::make('Welcome/SubPage');

$page->with('info', $info);

$page->withInfo($info);

$page->info = $info;

$page['info'] = $info;
```

To note the variable name transformation by dynamic **withX** methods.

Also, the View instances can be nested. The following commands are equivalent:

````php
// Add a View instance to a View's data
$view = View::make('foo')->nest('footer', 'Partials/Footer');

// Equivalent functionality using the "with" method
$view = View::make('foo')->with('footer', View::make('Partials/Footer'));
```

To note that nesting assumes that the nested View instance is a Standard View, not a Template one.

There is also a new **shares()** method, similar with actual **share()** but working for instances.

As rendering commands, we have:

* **fetch()** : will render the View and return the output

* **render()** : will render and output the View and display the output

* **display()** : same as display() but will send also the Headers.

Every Controller has now the ability to specify its Template and Layout, as following:

````php
class Welcome extends Controller
{
    protected $template = 'Admin';
    protected $layout = 'custom';

    /**
     * Call the parent construct
     */
    public function __construct()
    {
        parent::__construct();
        $this->language->load('Welcome');
    }

    ...
}
```

WHERE the '**default**' Layout is a simple composition of your **header/footer** files. For details, see: **app/Templates/Default/default.php**

### Advanced Usage

Rendering with partials (i.e. **header/footer**) from the standard Views location
If you need to work with partials, for example blocks, header and footer files located in **app/Views** directory, it is very simple to do that. You have just to compose your views as following:

The following examples use a very simple Template Layout file, called:

**app/Templates/Default/custom.php**

Rendering with a complete custom Template living on Views folder

````php
return Template::make('custom')
   ->shares('title', $title)
   ->with('header', View::make('Partials/Header'))
   ->with('content', View::make('Page/Index', $data))
   ->with('footer', View::make('Partials/Footer'));
```

OR

````php
return Template::make('custom')
   ->shares('title', $title)
   ->withHeader(View::make('Partials/Header'))
   ->withContent(View::make('Page/Index', $data))
   ->withFooter(View::make('Partials/Footer'));
```

OR

````php
return Template::make('custom')
   ->shares('title', $title)
   ->nest('header', 'Partials/Header')
   ->nest('content', 'Page/Index', $data)
   ->nest('footer', 'Partials/Footer');
```

OR

````php
return Template::make('custom')
   ->shares('title', $title)
   ->with('content', View::make('Partials/Layout')
   ->nest('content', 'Page/Index', $data));
```
Rendering in the Style, but using the new API

````php
return Template::make('custom')
   ->shares('title', $title)
   ->with('header', View::makeTemplate('header'))
   ->with('content', View::make('Page/Index', $data))
   ->with('footer', View::makeTemplate('footer'))
```

Rendering in the Style, but using some Views as blocks

````php
// Views instances automatically rendered into base View rendering.
$data['latestNewsBlock'] = View::make('Articles/LatestNewsBlock')->withNews($latestNews);
$data['topVisitedBlock'] = View::make('Articles/TopVisitedBlock')->withNews($topNews);

View::share('title', $title);

View::renderTemplate('header');
View::render('Welcome/Welcome', $data); // <-- there the View instances from $data will be fetched automatically
View::renderTemplate('footer');
```