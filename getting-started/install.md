- [Recommended](#recommended)
- [Manual](#manual)
- [XAMPP](#xampp)
- [Virtualhost](#virtualhost)
- [Nginx configuration](#nginx-configuration)
- [IIS with URL Rewrite module installed](#iis-with-url-rewrite-module-installed)


#### Recommended

This framework was designed and is strongly recommended to be installed above the document root directory, with it pointing to the public folder.

Additionally, installing in a sub-directory, on a production server, will introduce severe security issues. If there is no choice still place the framework files above the document root and have only index.php and .htacess from the public folder in the sub folder and adjust the paths accordingly.

The framework is located on [Packagist](https://packagist.org/packages/nova-framework/framework).

You can install the framework from a terminal by using:

```php
composer create-project nova-framework/framework foldername
```

The foldername is the desired folder to be created, if left off then a folder called framework will be created.

For a video example see [http://novacasts.com/courses/nova-fundamentals/install](http://novacasts.com/courses/nova-fundamentals/install)

---
#### XAMPP

Installing Nova-Framework 3 on xampp virtual server for Windows.

- Download composer installer for windows and run Composer-Setup.exe as you well need this to install the Nova Framework
package.
- Open terminal and run composer and if it installed correctly you will see a list of composer commands displayed.

You can install Nova two different ways.

The first way is under the C:/XAMPP/htdocs directory. (see further down on page for second way)

- Now in terminal go to the C:/XAMPP/htdocs directory.
- Run the below command replacing foldername with the name of your project.
```php
composer create-project nova-framework/framework foldername -s dev

- Now go to the folder with the name of your project created under the htdocs folder.

- Now lets create your virtualhost:

  - Open httpd-vhosts.conf file C:/XAMPP/apache/conf/extra/httpd-vhosts.conf Add following code.

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
    DocumentRoot "C:/XAMPP/htdocs/projectname/"
    ServerName projectname.dev
    SetEnv NS_ENV variable_value
</VirtualHost>
```

change projectname to you're projects name.

  - Save the file and exit out of Xampp.

  - Open hosts file in C:\windows\system32\drivers\etc you need Administrator privilege to edit the file.

  - Add 127.0.0.1    projectname.dev at the end of the file, Save and close the file.  Again changing projectname to your's.

```php
127.0.0.1       localhost
127.0.0.1       projectname.dev
```


- Restart Xampp and start apache server.
- Go to your project folder and open app/Config/App.php and change to your URL.

```php
    /**
     * The Website URL.
     */
    'url' => 'http://www.nova.dev/',
```


- now open browser and go to your url.  You should see the Nova startup screen.

**CONGRATS**, you're up and running.



The second way is to create a new folder like C:/vhosts.

- Now in terminal go to the C:/vhosts directory.

- Run the below command replacing foldername with the name of your project.
```php
composer create-project nova-framework/framework foldername -s dev
```


- - Now go to the folder with the name of your project created under the vhosts folder.

- Now lets create your virtualhost:

- Open httpd-vhosts.conf file C:/XAMPP/apache/conf/extra/httpd-vhosts.conf Add following code.

```php
<Directory C:/vhosts>
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
    DocumentRoot "C:/vhosts/projectname/"
    ServerName projectname.dev
    SetEnv NS_ENV variable_value
</VirtualHost>
```

change projectname to you're projects name.

- Save the file and exit out of Xampp.

- Open hosts file in C:\windows\system32\drivers\etc you need Administrator privilege to edit the file.

- Add 127.0.0.1 projectname.dev at the end of the file, Save and close the file.  Again changing projectname to your's.

```php
127.0.0.1       localhost
127.0.0.1       projectname.dev
```

- Restart Xampp and start apache server.

- Go to your project folder and open app/Config/App.php and change to your URL.
  
```php
    /**
     * The Website URL.
     */
    'url' => 'http://www.nova.dev/',
```

 - now open browser and go to your url.  You should see the Nova startup screen.

** CONGRATS**, you're up and running.



---

## VirtualHost

Navigate to: 

```php
<path to your xampp installation>\apache\conf\extra\httpd-vhosts.conf
```

and uncomment:

```php
NameVirtualHost *:80
```

Then add your VirtualHost to the same file at the bottom:

```php
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot "C:\xampp\testproject\public"
    ServerName testproject.dev

    <Directory "C:\xampp\testproject\public">
        Options Indexes FollowSymLinks Includes ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
```

Finally, find your hosts file and add:

```php
127.0.0.1       testproject.dev
```

You should then have a virtual host setup, and in your web browser, you can navigate to testproject.dev to see what you are working on.

---

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