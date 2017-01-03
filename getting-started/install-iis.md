IIS with URL Rewrite module installed - <a href='http://www.iis.net/downloads/microsoft/url-rewrite'>http://www.iis.net/downloads/microsoft/url-rewrite</a>

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
