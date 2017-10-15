In the event of an error or an exception, when in production mode a custom error message is displayed:

> Whoops! An error has occurred.

When in development mode a feature rich error page is show with a full stack trace of the route of the error.

The actual error is recorded in **storage/logs/error.log**. Any errors will be recorded in that file.

To change the development mode edit **webroot/index.php**

```php
//--------------------------------------------------------------------------
// APPLICATION ENVIRONMENT
//--------------------------------------------------------------------------
/*
You can load different configurations depending on your current environment.
Setting the environment also influences things like logging and error reporting.

This can be set to anything, but default usage is:

    development
    production
*/

define('ENVIRONMENT', 'development');
```
