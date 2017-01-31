- [Server Requirements](#server-requirements)
- [Install Composer](#install-composer)
- [Install Nova](#install-nova)
    - [Via Composer Create-Project](#via-composer-create-project)
    - [Via Download](#via-download)
    - [Local Development Server](#local-development-server)


This framework was designed and is strongly recommended to be installed above the document root directory, with it pointing to the public folder.

Additionally, installing in a sub-directory, on a production server, will introduce severe security issues. If there is no choice still place the framework files above the document root and have only index.php and .htacess from the public folder in the sub folder and adjust the paths accordingly.

<a name="server-requirements"></a>
## Server Requirements

You will need to make sure that your server meets the following requirements:

- PHP >=5.6
- Fileinfo PHP extension
- OpenSSL PHP extension
- INTL PHP extension

<a name="install-composer"></a>
## Install Composer

Nova utilizes [Composer](http://getcomposer.org/) to manage its dependencies. First, download a copy of the `composer.phar`. Once you have the PHAR archive, you can either keep it in your local project directory or move to  `usr/local/bin` to use it globally on your system. On Windows, you can use the Composer [Windows installer](https://getcomposer.org/Composer-Setup.exe).

<a name="install-nova"></a>
## Install Nova

<a name="via-composer-create-project"></a>
### Via Composer Create-Project

The framework is located on [Packagist](https://packagist.org/packages/nova-framework/framework).

You can install the framework from a terminal by using:

```php
composer create-project nova-framework/framework foldername 4.* -s dev
```

The `foldername` is the desired folder to be created.

If you want to use the framework without any added modules, you can do so by using:

```php
composer create-project nova-framework/app foldername 4.* -s dev
```

<a name="via-download"></a>
### Via Download

Once Composer is installed, download the [latest 4.0](https://github.com/nova-framework/framework/releases) version of the Nova framework and extract its contents into a directory on your server. Next, in the root of your Nova application, run the  `php composer.phar install` (or `composer install`) command to install all of the framework's dependencies. This process requires Git to be installed on the server to successfully complete the installation.

If you want to update the Nova framework, you may issue the `php composer.phar update` command.

<a name="local-development-server"></a>
### Local Development Server

If you have PHP installed locally and you would like to use PHP's built-in development server to serve your application, you may use the `serve` Forge command. This command will start a development server at http://localhost:8000:

```bash
php forge serve
```
