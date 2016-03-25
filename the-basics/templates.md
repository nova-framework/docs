Templates are the site's markup, where images and js,css files are located. The default template is called Default.

The structure of the default template is as follows

- Assets/css/style.css
- header.php
- footer.php

This is a very simple setup a regular site would have multiple css files and a javascript folder containing js files, an image folder. The rest of the sites pages come from the views.

Below is the contents of the header.php template, the title comes from an array with a key of title followed by the constant SITETITLE, this lets the controllers set the page titles in an array that is passed to the template.

<p>The url helper is being used to get the full path to the css file.</p>

````
<!DOCTYPE html>
<html lang="<?php echo LANGUAGE_CODE; ?>">
<head>
	<meta charset="utf-8">
	<title><?php echo $title.' - '.SITETITLE;?></title>
	<?php
    echo $meta;//place to pass data / plugable hook zone
    Assets::css([
        'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
        Url::templatePath().'css/style.css',
    ]);
    echo $css; //place to pass data / plugable hook zone
    ?>
</head>
<body>
<?php echo $afterBody; //place to pass data / plugable hook zone?>

<div class="container">
````
