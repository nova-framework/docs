# Upgrade

This guide will point out the key points to be aware of when upgrading to version 4.1.

All classes within the app directory have a new namespace of App.

Controllers, Models, Modules and Packages have the following namespaces for classes placed directly in those directories.

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
namespace Modules\ModuleName;
```

## Packages

```php
namespace Packages\VendorName\PackageName;
```

## Views

Views like classes should have filenames starting with a capital letter for both the directory and the file, for instance, welcome/index.php becomes Welcome/Index.php.

```php
View::make('Welcome/Index');
```

## Loading images, css, js and assets

Nova has been designed to live above the document root as such images and other assets cannot be called directly instead they need to be routed from Nova, this is done by calling `asset_url()`.

By default, this will return the path to the assets folder.

Make use of the Assets helper to load the CSS files:

```php
{{ Asset::render('css', array(
    'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
    asset_url('css/style.css')
));
```

An example of loading an image from Assets/images.

```php
<img src='<?=asset_url('images/nova.png');?>'>
```
