# Upgrade

This guide will point out the key points to be aware of when upgrading to version 4.2.

Carbon 1.x is now deprecated to Carbon 2.x Also, there's now a new file: `app/Routes/Assets.php` which expose the Assets Dispatcher routes.

Nova uses a secondary (and much smaller) Router for dispatching files from above of the site's document root, which is optimized for speed and the specific task of serving files. Customizing the `app/Routes/Assets.php` allows you to tune the files dispatching. The syntax used by the Assets Dispatcher is different from the one of main Router, it uses unnamed parameters like the ones which was used by SMVCF 2.2. Basically, its is as following:

```php
// Register the route for assets from main assets folder.
$dispatcher->route('assets/(:all)', function (Request $request, $path) use ($dispatcher)
{
    $basePath = Config::get('routing.assets.path', BASEPATH .'assets');

    $path = $basePath .DS .str_replace('/', DS, $path);

    return $dispatcher->serve($path, $request);
});

// Register the route for assets from Packages, Modules and Themes.
$dispatcher->route('packages/(:any)/(:any)/(:all)', function (Request $request, $vendor, $package, $path) use ($dispatcher)
{
    $namespace = $vendor .'/' .$package;

    return $dispatcher->servePackageFile($namespace, $path, $request);
});

// Register the route for assets from Vendor.
$dispatcher->route('vendor/(:all)', function (Request $request, $path) use ($dispatcher)
{
    return $dispatcher->serveVendorFile($path, $request);
});
```

An Assets Route could be defined using the method route() and it accepts two parameters: the first is the URI pattern, and the second one is always a Closure instance. This callback receives as first parameter the current Request instance, then the matched parameters using the unnamed style.

Why no named parameters in this Router? For speed reasons and simplicity. We should remember that this Assets Dispatcher/Router is executed always before the main Router, and on a single page loading it can receive dozens of requests for files.

If you already use an application using the latest Nova 4.1.x, the upgrade is relative simple, the changes being the used kernel version on composer.jon, on `app/Config/App.php` specially about the framework side's Service Providers and some changes on `App\Providers\RouteServiceProvider` and this new `app/Routes/Assets.php`


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

or using a .tpl view:

```php
<img src='{{{ asset_url('images/nova.png') }}}'>
```
