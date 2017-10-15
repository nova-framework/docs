[Recommended](#recommended)
[XAMPP](#xampp)
[Nginx configuration](#nginx-configuration)
[IIS with URL Rewrite module installed](#iis-with-url-rewrite-module-installed)

<a name='recommended'></a>
#### Recommended
This framework was designed and is strongly recommended to be installed above the document root directory, with it pointing to the webroot folder.

Additionally, installing in a sub-directory, on a production server, will introduce severe security issues. If there is no choice still place the framework files above the document root and have only index.php and .htacess from the webroot folder in the sub folder and adjust the paths in index.php.

```php
//path to the root folder holding all files
define('ROOTDIR', realpath(__DIR__.'/../') .DS);

//path to the app directory
define('APPDIR', realpath(__DIR__.'/../app/') .DS);

//path to the webroot directory
define('PUBLICDIR', realpath(__DIR__) .DS);
```

The framework is located on [Packagist](https://packagist.org/packages/nova-framework/framework).

You can install the framework from a terminal by using:

```php
composer create-project nova-framework/framework foldername 3.* -s dev
```

The foldername is the desired folder to be created.

> Notice the 3.* this mean install the latest version of the 3 range.

---
<a name='xampp'></a>
#### XAMPP

Installing Nova-Framework 3 on xampp virtual server for Windows.

The first way is under the `C:/XAMPP/htdocs` directory. (see further down on page for second way)

Now in command prompt go to the zC:/XAMPP/htdocs` directory.
Run the below command replacing foldername with the name of your project.

```php
composer create-project nova-framework/framework foldername 3.* -s dev
```

Now go to the folder with the name of your project created under the htdocs folder.
Open httpd-vhosts.conf file `C:/XAMPP/apache/conf/extra/httpd-vhosts.conf` Add following code.

```php
<Directory C:/XAMPP/htdocs>
    AllowOverride All
    Require all granted
</Directory>

#this is the default address of XAMPP
<VirtualHost *:80>
    DocumentRoot "C:/XAMPP/htdocs/"
    ServerName localhost
</VirtualHost>

#this is the vhost address in XAMPP
<VirtualHost *:80>
    DocumentRoot "C:/XAMPP/htdocs/projectname/webroot/"
    ServerName projectname.dev
    SetEnv NS_ENV variable_value
</VirtualHost>
```

change projectname to you're projects name.

Save the file and exit out of Xampp.
Open hosts file in `C:\windows\system32\drivers\etc` you need Administrator privilege to edit the file.
Add `127.0.0.1 projectname.dev` at the end of the file, Save and close the file. Again changing projectname to your's.

```php
127.0.0.1       localhost
127.0.0.1       projectname.dev
```

Restart Xampp and start apache server.
Go to your project folder and open `app/Config/App.php` and change to your URL.

```php
    'url' => 'http://www.projectname.dev/',
```

Now open browser and go to your url. You should see the Nova startup screen.


<a name='nginx-configuration'></a>
## Nginx configuration

No special configuration, you only need to configure Nginx and PHP-FPM.

```php
server {
  listen 80;
  server_name yourdomain.tld;

  access_log /var/www/access.log;
  error_log  /var/www/error.log;

  root   /var/www;
  index  index.php index.html;

  location = /robots.txt {access_log off; log_not_found off;}
  location ~ /\\. {deny all; access_log off; log_not_found off;}
  location / {
    try_files $uri $uri/ /index.php?$args;
  }

  location ~ \.php$ {
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
  }
}
```

---
<a name='iis-with-url-rewrite-module-installed'></a>
## IIS with URL Rewrite module installed

[http://www.iis.net/downloads/microsoft/url-rewrite](http://www.iis.net/downloads/microsoft/url-rewrite)

For IIS the htaccess needs to be converted to web.config:

```php
<configuration>
    <system.webserver>
        <directorybrowse enabled="true"/>
        <rewrite>
            <rules>
                <rule name="rule 1p" stopprocessing="true">
                    <match url="^(.+)/$"/>
                    <action type="Rewrite" url="/{R:1}"/>
                </rule>
                <rule name="rule 2p" stopprocessing="true">
                    <match url="^(.*)$"/
                    <action type="Rewrite" url="/index.php?{R:1}" appendquerystring="true"/>
                </rule>
            </rules>
        </rewrite>
    </system.webserver>
</configuration>
```
