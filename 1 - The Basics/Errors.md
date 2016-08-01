In the event of an error or an exception, a custom error message is displayed:

> An error occurred, The error has been reported to the development team and will be addressed asap.

This comes from the logger class in (app/core/logger.php) the actual error is recorded in errorlog.html located in the root of the framework, its advisable to move this above the public root. Any errors will be recorded in that file, ensuring no sensitive information it displayed on a page.

> This does not apply to fatal errors

To disable the custom error logger comment out the following in app/core/config.php

```php
set_exception_handler('logger::exception_handler');
set_error_handler('logger::error_handler');
```
