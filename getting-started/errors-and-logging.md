In the event of an error or an exception, when in production mode a custom error message is displayed:

> Whoops! An error has occurred.

When in development mode a feature rich error page is show with a full stack trace of the route of the error.

The actual error is recorded in **app/Storage/Logs/error.log**. Any errors will be recorded in that file.