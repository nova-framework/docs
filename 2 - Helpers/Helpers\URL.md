The URL class is used for having handy methods or redirecting the page and returning the path to the current template.

Redirect - To redirect to another page instead of using a header call the static method redirect:
```php
url::redirect('path/to/go');
```

> The redirect method can accept a 2nd option of true is used the path will be used as it is provided.
This is useful to redirect to an external website, by default the redirects are relative to the domain its on.

The url should be the local path excluding the application url for instance a valid case might be:
```php
url::redirect('contacts');
```
The redirect method uses the DIR constant to get the application address.

The next method is get_template_path, this returns the path to the template relative from the templates folder, for instance by default it will return: http://www.example.com/templates/default/ this is useful for using absolute paths in your design files such as including css and js files.
```php
url::get_template_path();
```
