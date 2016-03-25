Controllers are the bread and butter of the framework they control when a model is used and equally when to include a view for output. A controller is a class with methods, these methods are the outputted pages when used in conjunction with routes.

A method can be used for telling a view to show a page or outputting a data stream such as XML or a JSON array.  Put simply they are the logic behind your application.

Controllers can be placed in sub folders relative to the root of the Controllers folder, each class located in sub folders must have it's own namespace to for instance a sub folder called Admin and a class called Users should have a namespace of App\Controllers\Admin 

Controllers are created inside the app/Controllers folder. To create a controller, create a new file, the convention is to StudlyCaps without any special characters or spaces. Each word of the filename should start with a capital letter. For instance: AddOns.php.

Controllers will always use a namespace of App\Controllers, if the file is directly located inside the controllers folder. If the file is in another folder that folder name should be part of the name space.

For instance a controller called blog located in app/Controllers/Blog would have a namespace of App\Controllers\Blog</p>

Controllers need to use the main Controller; they extend it, the syntax is:

````
<?php 
namespace Controllers;

use Core\Controller

class Welcome extends Controller 
{

}
````

Also the view class is needed to include view files you can either call the namespace then the view:

Create an alias:

````
use Core\View;
````

Then to use:

````
View::render();
````

# Example 

````
<?php 
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
        $data['title'] = 'Welcome';

        View::renderTemplate('header', $data);
        View::render('welcome/welcome', $data);
        View::renderTemplate('footer', $data);
    }
}
````

Controllers will need to access methods and properties located in the parent controller (app/Core/Controller.php) in order to do this they need to call the parent constructor inside a construct method.

````
public function __construct()
{
    parent::__construct();
}
````

The construct method is called automatically when the controller is instantiated once called the controller can then call any property or method in the parent controller that is set as public or protected.

**The following property becomes available to the controller**

- $language / used to call the language object, useful when using language files

Both models and helpers can be used in a constructor and added to a property then becoming available to all methods. The model or helper will need to use its namespace while being called

````
<?php 
namespace App\Controllers;

use Core\View;
use Core\Controller;

class Blog extends Controller 
{
    private $blog;

    public function __construct()
    {
        parent::__construct();
        $this->blog = new \Models\\Blog();
    }

    public function blog()
    {
        $data['title'] = 'Blog';
        $data['posts'] = $this->blog->getPosts();

        View::render('blog/posts', $data);
    }
}
````

## Methods:

To use a model in a controller, create a new instance of the model. The model can be placed directly in the models folder or in a sub folder, For example: 

````
public function index()
{
    $data = new \Model\Classname();
}
````

## Helpers:

A helper can be placed directly in the helpers folder or in a sub folder.

````
public function index()
{
    //call the session helper
    Session::set('username', 'Dave');
}
````

Load a view, by calling its render method, pass in the path to the file inside the views folder. 

````
use Core\View;

public function index()
{
    //static way
    View::render('welcome/welcome');
}
````

A controller can have many methods, a method can call another method, all standard OOP behaviour is honoured.
Data can be passed from a controller to a view by passing an array to the view.
The array can be made up from keys. Each key can hold a single value or another array.
The array must be passed to the method for it to be used inside the view page or in a template (covered in the templates section)

````
$data['title'] = 'My Page Title';
$data['content'] = 'The contact for the page';
$data['users'] = array('Dave', 'Kerry', 'John');

View::render('contacts', $data);
````

Using a model is very similar, an array holds the results from the model, the model calls a method inside the model.

````
$contacts = new \Models\Contacts();
$data['contacts'] = $contacts->getContacts();
````
