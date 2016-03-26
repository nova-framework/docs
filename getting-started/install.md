# Install

The framework is now on packagist [https://packagist.org/packages/simple-mvc-framework/v2](https://packagist.org/packages/simple-mvc-framework/v2)

Install from terminal now by using:

````
composer create-project simple-mvc-framework/v2 foldername -s dev  
````

The foldername is the desired folder to be created.
<br>

##Install Manually

Option 1 - files above document root:

* place the contents of public into your public folder (.htaccess and index.php)
* navigate to your project in terminal and type composer install to initiate the composer install.
* edit public/.htaccess set the rewritebase if running on a sub folder otherwise a single / will do.
* edit app/Config.example.php change the SITEURL and DIR constants. the DIR path this is relative to the project url for example / for on the root or /foldername/ when in a folder. Also change other options as desired. Rename file as Config.php

Option 2 - everything inside your public folder

* place all files inside your public folder 
* navigate to the public folder in terminal and type composer install to initiate the composer install.
* open index.php and change the paths from using DIR to FILE:

````
define('APPDIR', realpath(__DIR__.'/app/').'/');
define('SYSTEMDIR', realpath(__DIR__.'/system/').'/');
define('PUBLICDIR', realpath(__DIR__).'/');
define('ROOTDIR', realpath(__DIR__).'/');
````

* edit .htaccess set the rewritebase if running on a sub folder otherwise a single / will do.
* edit system/Core/Config.example.php change the SITEURL and DIR constants. the DIR path this is relative to the project url for example / for on the root or /foldername/ when in a folder. Also change other options as desired. Rename file as Config.php

---
<br>

##Nginx configuration

No special configuration, you only need to configure Nginx and PHP-FPM.

````
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
````
  
---
<br>

##IIS with URL Rewrite module installed - [http://www.iis.net/downloads/microsoft/url-rewrite](http://www.iis.net/downloads/microsoft/url-rewrite)  

For IIS the htaccess needs to be converted to web.config:  

````
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
````
