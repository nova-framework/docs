# File

File service is a collection of useful file related function.

In a controller the file will need importing: 

```php
use File;
```

In a view it's already included in **app/Config/App.php**

```php
File::exists($path);
```

Determine is the file path exists.

```php
File::get($path);
```

Gets the content of a file

```php
File::getRequire($path);
```

Get the returned value of a file.

```php
File::requireOnce($file);
```

```php
put($path, $contents, $lock = false)
```

Write the contents of a file.

Require the given file once.
