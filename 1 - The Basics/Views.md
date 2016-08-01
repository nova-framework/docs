Views are the visual side of the framework, they are the html output of the pages. All views are located in the views folder. The actual views can be located directly inside the views folder or in a sub folder, this helps with organising your views.

Views are called from controllers once called they act as include files outputting anything inside of them. They have access to any data passed to them from a data array.

The parent view class has two methods render and rendertemplate:

```php
class View {

  public function render($path,$data = false, $error = false){
    require "views/$path.php";
  }

  public function rendertemplate($path,$data = false){
    require "templates/".Session::get('template')."/$path.php";
  }

}
```

The render method is used to include a view file, the method expects the path to the view. Optionally an array can be passed as well as an error array, this is useful for passing in errors for validation in a controller.

The rendertemplate is almost the same except its use is for including templates, useful for including header and footer files for your application's design.

The template folder used is dictated by the template set in the app/core/config file via a session.

**Using a view from a controller**

A view can be set inside a method, an array can optionally be created and passed to both the render and rendertemplate methods, this is useful for setting the page title and letting a header template use it.

```php
$data['title'] = 'Welcome';

$this->view->rendertemplate('header',$data);
$this->view->render('welcome/welcome',$data);
$this->view->rendertemplate('footer',$data);
```

**Inside a view**

Views are normal php files they can contain php and html, as such any php logic can be used inside a view though its recommended to use only simple logic inside a view anything more complex is better suited inside a controller.

An example of a view; looping through an array and outputting its contents:

```php

// Contacts List

if($data['contacts']){
  foreach($data['contacts'] as $row){
    echo $row.'<br />';
  }
}
```
