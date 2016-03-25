A language class exists inside the system/Core folder, this class have 2 methods:

1. load - Loads the language file, can return the data and set the language to use
2. get - return an language string if it exists, else it will return the value passed

Inside the system/Core/Controller.php file the class is instantiated, resulting in $this->language being available to all controllers.

To use a language inside a controller use the following passing the file to be loaded located in the language/code/filename.php by default the language code will be en.

````
$this->language->load('file/to/load');
````

The load method can also be passed if the data is to be returned with a true or false and the language code, useful to set a new language on the call:

````
$this->language->load('file/to/load', false, 'nl');
````

The default language can be set in the Config.php file:

````
//set a default language
define('LANGUAGE_CODE', 'en');
````

Inside the language file set the text, each language should contain the same text in their own language for instance:

````
//en
$lang['welcomeMessage'] = 'Hello, welcome from the welcome controller!';
````

````
//nl
$lang['welcomeMessage'] = 'Hallo, welkom van de welcome controller!';
````

To use the language strings inside a controller, set a data array and call the get method passing the desired string to return:

````
$data['welcomeMessage'] = $this->language->get('welcome_message');
````

Then in the view echo $data['welcomeMessage'] to print the chosen language.

Welcome example

````
<?php 
namespace App\Controllers;

use Core\View;
use Core\Controller;

class Welcome extends Controller
{
    /**
     * call the parent construct
     */
    public function __construct()
    {
        parent::__construct();
        $this->language->load('Welcome');
    }

    /**
     * define page title and load template files
     */
    public function index()
    {
        $data['title'] = 'Welcome';
        $data['welcomeMessage'] = $this->language->get('welcomeMessage');

        View::rendertemplate('header', $data);
        View::render('welcome/welcome', $data);
        View::rendertemplate('footer', $data);
    }

}
````
