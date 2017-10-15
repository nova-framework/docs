# Upgrade

This guide will point out the key points to be aware of when upgrading to version 3.

All classes within the app directory have a new namespace of App.

Controllers, Models and Modules have the following namespaces for classes placed directly in those directories.

## Controllers

```php
namespace App\Controllers;
```

## Models

```php
namespace App\Models;
```

## Modules

```php
namespace App\Controllers\ModuleName;
```

## Views

Views like classes should have filenames starting with a capital letter for both the directory and the file, for instance, welcome/index.php becomes Welcome/Index.php.

```php
View::make('Welcome/Index');
```

## Loading images, css, js and assets

Nova has been designed to live above the document root as such images and other assets cannot be called directly instead they need to be routed from Nova, this is done by calling `resource_url()`.

By default, this will return the path to the assets folder.

For theme files place them inside the Theme/Assets directory and call them by using `theme_url()` this will load the path to the theme.

Make use of the Assets helper to load the CSS files:

```php
Assets::css([
    'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
    theme_url('css/style.css', 'Bootstrap')
]);
```

An example of loading an image from Bootstrap/Assets/images.

```php
<img src='<?=theme_url('images/nova.png', 'Bootstrap');?>'>
```
