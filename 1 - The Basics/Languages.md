A new feature written by <a href='http://qsma.pl'>Bartek Kusmierczuk</a> is a language component, this allows for different languages to be supported easily.

A language class exists inside the core folder, this class have 2 methods:


- load - Loads the language file, can return the data and set the language to use
- get - return an language string if it exists, else it will return the value passed

Inside the Core/Controller.php file the class is instantiated, resulting in $this->language being available to all controllers.

To use a language inside a controller use the following passing the file to be loaded located in the language/code/filename.php by default the language code will be en.

```
$this->language->load('file/to/load');
```

The load method can also be passed if the data is to be returned with a true or false and the language code, useful to set a new language on the call:

```
$this->language->load('file/to/load',false,'nl');
```

The default language can be set in the Config.php file:

```
//set a default language
define('LANGUAGE_CODE', 'en');
```

Inside the language file set the text, each language should contain the same text in their own language for instance:

```
//en
$lang['welcome_message'] = 'Hello, welcome from the welcome controller!';
```

```
//nl
$lang['welcome_message'] = 'Hallo, welkom van de welcome controller!';
```

To use the language strings inside a controller, set a data array and call the get method passing the desired string to return:

```
$data['welcome_message'] = $this->language->get('welcome_message');
```

Then in the view echo $data['welcome_message'] to print the chosen language.

# Welcome example

```
<?php
namespace Controllers;

use Core\\View;
use Core\\Controller;

class Welcome extends Controller
{
    /**
     * call the parent construct
     */
    public function __construct()
    {
        parent::__construct();
        $this->language->load('welcome');
    }

    /**
     * define page title and load template files
     */
    public function index()
    {
        $data['title'] = 'Welcome';
        $data['welcome_message'] = $this->language->get('welcome_message');

        View::rendertemplate('header', $data);
        View::render('welcome/welcome', $data);
        View::rendertemplate('footer', $data);
    }

}
```
