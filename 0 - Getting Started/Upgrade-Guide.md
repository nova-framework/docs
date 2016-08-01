This guide will point out the key points to be aware of when upgrading to version 3.

All classes within the app directory have a new namespace of App, controllers and models/modules have the following namespaces for classes placed directly in those directories.

## Controllers
````php
namespace App\Controllers;
```

## Models
````php
namespace App\Models;
```

## Modules
````php
namespace App\Controllers\ModuleName;
```

## Instantiating a model

Models are instantiated typically within a construct method, use the full namespace to call the class:
````php
public function __construct()
{
    parent::__construct();
    $this->model = new \App\Models\ModelName();
}
```

## Views

Views like classes should have filenames starting with a capital letter for both the directory and the file, for instance, welcome/index.php becomes Welcome/Index.php.

````php
View::render('Welcome/Index', $data);
```

## Loading Images, CSS, js, and assets

Nova has been designed to live above the document root as such images and other assets cannot be called directly instead they need to be routed from Nova, this is done by calling Url::resourcePath().

By default, this will return the path to the assets folder, place general assets in there. For theme files place them inside the Theme/Assets directory and call them by using Url::templatePath() this will load the path to the default template.

Make use of the Assets helper to load the CSS files:
````php
Assets::css([
    'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
    Url::templatePath().'css/style.css'
]);
```

An example of loading an image from Default/Assets/images.
````php
<img src='<?=Url::templatePath();?>images/nova.png' alt='<?=SITETITLE;?>'>
```