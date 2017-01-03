Templates are the site's markup, where images and js,css files are located. The default template is called default.

The structure of the default template is as follows:

- css/style.css
- header.php
- footer.php

This is a very simple setup a regular site would have multiple css files and a javascript folder containing js files, an image folder. The rest of the sites pages come from the views.

Below is the contents of the header.php template, the title comes from an array with a key of title followed by the constant SITETITLE, this lets the controllers set the page titles in an array that is passed to the template

The url helper is being used to get the full path to the css file.

```
<?php

use Helpers\\Assets;
use Helpers\\Url;

?>
<!DOCTYPE html>
<html lang="<?php echo LANGUAGE_CODE; ?>">
<head>

    <!-- Site meta -->
    <meta charset="utf-8">
    <title><?php echo $data['title'].' - '.SITETITLE; //SITETITLE defined in app/Core/Config.php ?></title>

    <!-- CSS -->
    <?php
    Assets::css(array(
        '//maxcdn.bootstrapcdn.com/bootstrap/3.3.1/css/bootstrap.min.css',
        Url::templatePath() . 'css/style.css',
    ));
    ?>

</head>
<body>
```
