Views are the visual side of the Nova, they are the HTML output of the pages. Views can be located directly inside the **app/Views** folder or in a sub folder, this helps with organising your views.

Views are called from controllers once called they act as included files outputting anything inside of them. They have access to any data passed to them.

### Using a view from a controller

To load a view in a controller call **return View::make()** it's important return is used without it the system Middleware will not be called and no view will be loaded. 

View::make accepts a path to the view relative to the **app/Views** folder. 
The second param is the data and the third param is the name of the module when loading module views.

Import the View:

```php
use View;
```

Return the view

```php
return View::make('Welcome/Welcome')->shares('title', 'Welcome');
```

To load a view from a module pass the module name as a third parameter.

```php
return View::make('Posts/Post', ['posts' => $posts], 'Modulename');
```

In cases when you only need to load a view from a module the second param should be an empty array like this:

```php
return View::make('Post/Post', [], 'Blog');
```

The **Recommended** way to reutn a view is to call **return $this->getView()**
This will automatically determine the path and view file to be loaded based on the controller and method used.

For instance:

```php
class Welcome extends Controller
{
    public function index()
    {
        return $this->getView()->shares('title', 'Welcome');
    }
}
```

This snippet is from the Welcome controller, from this Nova know's to load **app/Views/Welcome/Index.tpl** this worked out automatically when using this approach a folder matching the controller and method should exist in the Views folder. 

For Modules view folders should be placed inside **app/Modules/ModuleName/Views/Controllername/Viewfile.tpl**

For example a module called Blog has a method called post would have a view path of **app/Modules/Blog/View/Blog/Post.tpl**

## .php or .tpl

Views can use either .php for normal php files or .tpl should be used for views using the template engine. ie when you want to use template tags like {{{ $title }}} use .tpl

## Inside a view

Views are normal PHP files / template files (.tpl), they can contain PHP and HTML, as such any PHP logic can be used inside a view though it's recommended to use only simple logic inside a view anything more complex is better suited inside a controller.

views with .tpl should not contain any way php use .php files or that or wrap them inside:

```php
@php
//php code
@endphp
```
For more on this please see the Template Engine page.

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
2. The data being passed must be an array
3. Optional when loading a view from a module pass in the module name.

```php
$content = View::fetch('Page/Show', ['content' => $content], 'Pages');

echo $content;
```

Alternative **View/Layout** options:

### Basic Commands

While the actual **View\View** methods are static and they should call independently, the new API works with View instances, then we should build them. We have two methods for standard Views and Themes files. A combined usage example is presented below:

```php
return View::make('Welcome/SubPage')
    ->shares('title', $title)
    ->with('data', $data);

// OR

$page = View::make('Welcome/SubPage')->with('data', $data);

return View::makeLayout('default')
    ->shares('title', $title)
    ->with('page' $page);
```

View Methods can be chained.
**shares()** is a way to share a variable that is accessible to the theme files, useful for settings the page title.
to pass data to the view files use ->with() command. With accepts 2 params:
1. the variable name to set
2. the value

Another way to set the variable is to add the name to end of ->with for example to pass a variable called contacts: 

```php
->withContacts($data)
```

To note the variable name transformation by dynamic **withX** methods.

Also, the View instances can be nested. The following commands are equivalent:

```php
// Add a View instance to a View's data
$view = View::make('foo')->shares('title', 'title')->nest('footer', 'Partials/Footer');

// Equivalent functionality using the "with" method
$view = View::make('foo')->shares('title', 'title')->with('footer', View::make('Partials/Footer'));
```

To note that nesting assumes that the nested View instance is a Standard View, not a Theme one.

There is also a new **shares()** method, similar with actual **share()** but working for instances.

Every Controller has now the ability to specify its Theme and Layout file, as following:

```php
class Welcome extends Controller
{
    protected $theme = 'Admin';
    protected $layout = 'custom';

}
```

WHERE the '**default**' Layout is a simple composition of your **header/footer** files. For details, see: **app/Templates/Bootstrap/default.php**

### Advanced Usage

Rendering with partials (i.e. **header/footer**) from the standard Views location
If you need to work with partials, for example blocks, header and footer files located in **app/Views** directory, it is very simple to do that. You have just to compose your views as following:

The following examples use a very simple Theme Layout file, called:

**app/Themes/Bootstrap/custom.php**

Rendering with a complete custom Template living on Views folder

```php
return View::makeLayout('custom')
   ->shares('title', $title)
   ->with('header', View::make('Partials/Header'))
   ->with('content', View::make('Page/Index', $data))
   ->with('footer', View::make('Partials/Footer'));
```

OR

```php
return View::makeLayout('custom')
   ->shares('title', $title)
   ->withHeader(View::make('Partials/Header'))
   ->withContent(View::make('Page/Index', $data))
   ->withFooter(View::make('Partials/Footer'));
```

OR

```php
return View::makeLayout('custom')
   ->shares('title', $title)
   ->nest('header', 'Partials/Header')
   ->nest('content', 'Page/Index', $data)
   ->nest('footer', 'Partials/Footer');
```

OR

```php
return View::makeLayout('custom')
   ->shares('title', $title)
   ->with('content', View::make('Partials/Layout')
   ->nest('content', 'Page/Index', $data));
```
Rendering in the Style, but using the new API

```php
return View::makeLayout('custom')
   ->shares('title', $title)
   ->with('header', View::makeTemplate('header'))
   ->with('content', View::make('Page/Index', $data))
   ->with('footer', View::makeTemplate('footer'))
```
