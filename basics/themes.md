themes are the site's markup, where images, js, and css files are located as well as the site html structure. The default template is called Bootstrap.

A regular site would have multiple css files and a javascript folder containing js files, an image folder. The rest of the sites pages come from the views.

The typical elements of a page includes the following. The title comes **View::shares** set in controllers, this lets the controllers set the page titles in an array that is passed to the theme. The site title **Config::get('app.name', SITETITLE)** comes from app/Config/App.php 

# Routing images / js / css files
From within Themes your css, js and images must be in a Assets folder to be routed correctly. This applies to Modules as well, to have a css file from a Module the css file would be placed inside **app/Modules/ModuleName/Assets/css/file.css.** Additionally there is an Assets folder in the root of nova this is for storing resources outside of themes that can still be routed from above the document root.

Routing CSS:

>**Note** Theme folder names less then 4 characters should be in capital letters ie **Scf** should be **SCF** or resources will not be found.

CSS files can be loaded by using Assets::css() and passing in an array of css paths.

site_url() is used to determine the website url. Place paths to the files relative to the root.

them_url() is used to load resources from the theme. It accepts two params:
1 - path to the css file relative to the theme's root.
2 - the theme name to be used

```php
Assets::css([
    //load from vendor
    site_url('vendor/twbs/bootstrap/dist/css/bootstrap.min.css'),
    site_url('vendor/twbs/bootstrap/dist/css/bootstrap-theme.min.css'),
    //external
    'https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css',
    //load from template
    theme_url('css/style.css', 'Bootstrap'),
]);
```

To load JS is the same process only this time its Assets::js

```php
Assets::js([
    //external
    'https://code.jquery.com/jquery-1.12.4.min.js',
    //vendor
    site_url('vendor/twbs/bootstrap/dist/js/bootstrap.min.js'),
]);
```

Loading images, since the images are above the document root they have to be served from Nova, this is done by either using theme_url() for images in the theme or resource_url() for resources in the global assets folder or from a module.

theme_url() accepts two params:
1 - path to the image file relative to the theme's root.
2 - the theme name to be used

```php
<img src='<?= theme_url('images/nova.png', 'Default'); ?>' alt='logo'>
```

For images, js and css files located in /assets
resource_url() accepts two params:
1 - path to the resource
2 - optionally the name of the module

```php
<img src='<?= resource_url('images/nova.png', 'Default'); ?>' alt='logo'>
```


Page example:

```php
<?php
/**
 * Default Layout - a Layout similar with the classic Header and Footer files.
 */

// Generate the Language Changer menu.
$language = Language::code();

$languages = Config::get('languages');

//
ob_start();

foreach ($languages as $code => $info) {
?>
<li <?php if($language == $code) echo 'class="active"'; ?>>
    <a href='<?= site_url('language/' .$code); ?>' title='<?= $info['info']; ?>'><?= $info['name']; ?></a>
</li>
<?php
}

$langMenuLinks = ob_get_clean();
?>
<!DOCTYPE html>
<html lang="<?= $language; ?>">
<head>
    <meta charset="utf-8">
    <title><?= $title .' - ' .Config::get('app.name', SITETITLE); ?></title>
<?php
echo isset($meta) ? $meta : ''; // Place to pass data / plugable hook zone

Assets::css([
    vendor_url('dist/css/bootstrap.min.css', 'twbs/bootstrap'),
    vendor_url('dist/css/bootstrap-theme.min.css', 'twbs/bootstrap'),
    'https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css',
    theme_url('css/style.css', 'Bootstrap'),
]);

echo isset($css) ? $css : ''; // Place to pass data / plugable hook zone
?>
</head>
<body style='padding-top: 28px;'>

<nav class="navbar navbar-default navbar-xs navbar-fixed-top" role="navigation">
    <div class="container-fluid">
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav navbar-right">
                <?= $langMenuLinks; ?>
            </ul>
        </div>
    </div>
</nav>

<?= isset($afterBody) ? $afterBody : ''; // Place to pass data / plugable hook zone ?>

<div class="container">
    <p>
        <img src='<?= theme_url('images/nova.png', 'Bootstrap'); ?>' alt='<?= Config::get('app.name', SITETITLE); ?>'>
    </p>

    <?= $content; ?>
</div>

<footer class="footer">
    <div class="container-fluid">
        <div class="row" style="margin: 15px 0 0;">
            <div class="col-lg-4">
                <p class="text-muted">Copyright &copy; <?php echo date('Y'); ?> <a href="http://www.novaframework.com/" target="_blank"><b>Nova Framework <?= $version; ?> / Kernel <?= VERSION; ?></b></a></p>
            </div>
            <div class="col-lg-8">
                <p class="text-muted pull-right">
                    <?php if(Config::get('app.debug')) { ?>
                    <small><!-- DO NOT DELETE! - Profiler --></small>
                    <?php } ?>
                </p>
            </div>
        </div>
    </div>
</footer>

<?php
Assets::js([
    'https://code.jquery.com/jquery-1.12.4.min.js',
    vendor_url('dist/js/bootstrap.min.js', 'twbs/bootstrap'),
]);

echo isset($js) ? $js : ''; // Place to pass data / plugable hook zone

echo isset($footer) ? $footer : ''; // Place to pass data / plugable hook zone
?>

<!-- DO NOT DELETE! - Forensics Profiler -->

</body>
</html>
```

The default theme comes with 2 layout files to demonstrate different use cases. 

default.php is the full page layouts 
default-rtl.php mean they are right to left formats.

A theme can be as simple as a single layout file and an assets folder and message.php file.
