Controllers are the bread and butter of the framework they control when a model is used and equally when to include a view for output. A controller is a class with methods, these methods are the outputted pages when used in conjunction with routes.

A method can be used for telling a view to show a page or outputting a data stream such as XML or a JSON array. Put simply they are the logic behind your application.

Controllers can be placed in subfolders relative to the root of the Controllers folder, each class located in subfolders must have its own namespace to for instance a subfolder called Admin and a class called Users should have a namespace of **App\Controllers\Admin**

Controllers are created inside the **app/Controllers** folder. To create a controller, create a new file, the convention is to **StudlyCaps** without any special characters or spaces. Each word of the filename should start with a capital letter. For instance: **AddOns.php**.

Controllers will always use a namespace of **App\Controllers**, if the file is directly located inside the controllers folder. If the file is in another folder that folder name should be part of the name space.

For instance, a controller called blog located in **app/Controllers/Blog** would have a namespace of **App\Controllers\Blog**

Controllers need to use the main Controller; they extend it, the syntax is:
````php
namespace App\Controllers;

use Core\Controller

class Welcome extends Controller 
{

}
```

Also, the view class is needed to include view files, you can either call the namespace then the view:

Create an alias:
````php
use Core\View;
```
Then to use:

````php
return View::make('path);
```

**Example**

````php

namespace App\Controllers;

use Core\View;
use Core\Controller;

class Welcome extends Controller
{
    public function __construct()
    {
        parent::__construct();
    }

    public function index()
    {   
        return View::make('Welcome/Welcome')->shares('title', 'Welcome);
    }
}
```

Controllers will need to access methods and properties located in the parent controller (**app/Core/Controller.php**) in order to do this they need to call the parent constructor inside a construct method.

````php
public function __construct()
{
    parent::__construct();
}
```

The construct method is called automatically when the controller is instantiated once called the controller can then call any property or method in the parent controller that is set as public or protected.

The following property becomes available to the controller

* $language / used to call the language object, useful when using language files

Both models and helpers can be used in a constructor and added to a property then becoming available to all methods. The model or helper will need to use its namespace while being called
````php
namespace App\Controllers;

use Core\View;
use Core\Controller;

class Blog extends Controller 
{
    private $blog;

    public function __construct()
    {
        parent::__construct();
        $this->blog = new \App\Models\Blog();
    }

    public function blog()
    {
        $posts = $this->blog->getPosts();

        View::make('Blog/Posts')->shares('title', 'Blog')->withPosts($posts);
    }
}
```

An alternative way to load a view is to call $this->getView() this acts in the same way as View::make() only without the passed params, the path is worked out internally. 

To use getView the view path should follow the controller and method name ie:

````php
class Users extends Controller
{
    public function index()
    {
      return $this->getViews()->shares('title', 'The Title');
    }
}

Will match to app/Views/Users/Index.php
```

### Methods
To use a model in a controller, create a new instance of the model. The model can be placed directly in the models folder or in a sub folder, For example:

````php
public function index()
{
    $data = new \App\Models\Classname();
}
```

### Helpers
A helper can be placed directly in the helpers folder or in a sub folder.

````php
public function index()
{
    //call the session helper
    Session::set('username', 'Dave');
}
```

Load a view, by calling its render method, pass in the path to the file inside the views folder.

````php
use Core\View;

public function index()
{
    //static way
    View::make('Welcome/Welcome');
}
```

A controller can have many methods, a method can call another method, all standard OOP behaviour is honoured. Data can be passed from a controller to a view by passing an array to the view. The array can be made up from keys. Each key can hold a single value or another array. The array must be passed to the method for it to be used inside the view page or in a template (covered in the templates section)

````php

$content = 'The contact for the page';
$users = array('Dave', 'Kerry', 'John');

View::make('Contacts/Contacts')->withContent($content)->withUsers($users);
```

Using a model is very similar, an array holds the results from the model, the model calls a method inside the model.

````php
$contacts = new \App\Models\Contacts();
$data['contacts'] = $contacts->getContacts();
```