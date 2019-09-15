Views are the visual side of the Nova, they are the HTML output of the pages. Views can be located directly inside the **app/Views** folder or in a sub folder, this helps with organising your views.

Views are called from controllers once called they act as included files outputting anything inside of them. They have access to any data passed to them.

### Using a view from a controller

To load a view in a controller call **return View::make()** it's important return is used without it the system Middleware will not be called and no view will be loaded. 

View::make accepts a path to the view relative to the **app/Views** folder. 
The second param is the data and the third param is the name of the module when loading module views.

Import the View:

```php
use Nova\Support\Facades\View;
```

Return the view

```php
return View::make('Posts/Index')->shares('title', 'Welcome');
```

To load a view and pass a vaiable

```php
$post = Post::find($id);
return View::make('Posts/Post', compact('post'));
```

## .php or .tpl

Views can use either .php for normal php files or .tpl should be used for views using the template engine. ie when you want to use template tags like {{{ $title }}} use .tpl

## Inside a view

Views are normal PHP files / template files (.tpl), they can contain PHP and HTML, as such any PHP logic can be used inside a view though it's recommended to use only simple logic inside a view anything more complex is better suited inside a controller.

views with .tpl should not contain large php blocks but for small items you can wrap the code around @php and @endphp blocks:

```php
@php
//php code
@endphp
```
For more on this please see the Template Engine page.

An example of a view; looping through an array and outputting its contents:

```php
 <p>Contacts List</p>
 @if ($contacts) 
    @foreach ($contacts as $contact) 
        {{{ $contact.'<br />' }}};
    @endforeach
@endif
```

To return a view and store its contents use View::fetch, fetch takes 2 params:
1. The view path relative to the view folder or module
2. The data being passed must be an array

```php
$content = View::fetch('Page/Show', compact('content'));

echo $content;
```

Alternative **View/Layout** options:

### Basic Commands

While the actual **View\View** methods are static and they should be called independently, the API works with View instances. We have two methods for standard Views and Themes files. A combined usage example is presented below:

```php
return View::make('Welcome/SubPage', compact('data'))
    ->shares('title', $title);
```
