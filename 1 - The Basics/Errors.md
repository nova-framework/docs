In the event of an error or an exception, a custom error message is displayed:

> An error occurred, The error has been reported.

This comes from the logger class in (**system/Core/Logger.php**) the actual error is recorded in **app/Storage/Logs/error.log**. Any errors will be recorded in that file, ensuring no sensitive information it displayed on a page.

To loop through and display errors without doing it yourself call the display method of the error class:

````php
echo Errors::display($error);
```