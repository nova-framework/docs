Views are the visual side of the framework, they are the html output of the pages. All views are located in the views folder. The actual views can be located directly inside the views folder or in a sub folder, this helps with organising your views.

Views are called from controllers once called they act as included files outputting anything inside of them. They have access to any data passed to them from a data array.

The parent view class has two methods render and renderTemplate:

```
<?php
namespace Core;

use Helpers\\Session;

class View
{
    public static function render($path, $data = false, $error = false)
    {
        if (!headers_sent()) {
            foreach (self::$headers as $header) {
                header($header, true);
            }
        }
        require "app/views/$path.php";
    }

    public static function renderTemplate($path, $data = false, $custom = false)
    {
        if (!headers_sent()) {
            foreach (self::$headers as $header) {
                header($header, true);
            }
        }

        if ($custom == false) {
            require "app/templates/".TEMPLATE."/$path.php";
        } else {
            require "app/templates/$custom/$path.php";
        }
    }

}
```

The render method is used to include a view file, the method expects the path to the view. Optionally an array can be passed as well as an error array, this is useful for passing in errors for validation in a controller.

The renderTemplate is almost the same except its use is for including templates, useful for including header and footer files for your application's design. The template defined inside the TEMPLATE constant is used by default but passing a third parameter to renderTemplate containing a string can be used to use a different template folder.

For example, calling an email template can be done like this:

```
View::renderTemplate('header', $data, 'email');
```

The template folder used is dictated by the template set in the app/Core/Config file via a constant.

<b>Using a view from a controller</b>

A view can be set inside a method, an array can optionally be created and passed to both the render and renderTemplate methods, this is useful for setting the page title and letting a header template use it.

```
 $data['title'] = 'Welcome';

 View::renderTemplate('header', $data);
 View::render('welcome/welcome', $data);
 View::renderTemplate('footer', $data);
```

<b>Inside a view</b>

Views are normal php files they can contain php and html, as such any php logic can be used inside a view though it's recommended to use only simple logic inside a view anything more complex is better suited inside a controller.

An example of a view; looping through an array and outputting its contents:

```
 Contacts List
 <?php
 if ($data['contacts']) {
    foreach ($data['contacts'] as $row) {
        echo $row.'<br />';
    }
 }
 ?>
```
