# Errors

In the event of an error or an exception, a custom error message is displayed:

> An error occurred, The error has been reported to the development team and will be addressed asap. 

his comes from the logger class in (app/Core/Logger.php) the actual error is recorded in errorlog.html located in the root of the framework, it's advisable to move this above the public root. Any errors will be recorded in that file, ensuring no sensitive information it displayed on a page.

> This does not apply to fatal errors

To disable the custom error logger comment out the following in app/Core/Config.php

````
set_exception_handler('Logger::exceptionHandler');
set_error_handler('Logger::errorHandler');
````

To loop through and display errors without doing it yourself call the display method of the error class:

````
echo Error::display($error); 
````