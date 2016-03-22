# Install

The framework is now on packagist [https://packagist.org/packages/simple-mvc-framework/v2](https://packagist.org/packages/simple-mvc-framework/v2)

Install from terminal now by using:

```
composer create-project simple-mvc-framework/v2 foldername -s dev
```

The foldername is the desired folder to be created.

1.  Download the framework
2.  Unzip the package.
3.  To run composer, navigate to your project on a terminal/command prompt then run ‘composer install’ that will update the vendor folder. Or use the vendor folder as is (composer is not required for this step)
4.  Upload the framework files to your server. Normally the index.php file will be at your root.
5.  Open the app/Core/routes.php file with a text editor, setup your routes.
6.  Open app/Core/Config.example.php and set your base path (if the framework is installed in a folder the base path should reflect the folder path /path/to/folder/ otherwise a single / will do. and database credentials (if a database is needed). Set the default theme. When you are done, rename the file to Core/Config.php
7.  Edit .htaccess file and save the base path. (if the framework is installed in a folder the base path should reflect the folder path /path/to/folder/ otherwise a single / will do.
