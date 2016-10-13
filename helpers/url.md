#**From v3.73.3 this has been removed**

The URL class is used for having handy methods or redirecting the page and returning the path to the current template.

**Redirect** - To redirect to another page instead of using a header call the static method redirect:

```php
Url::redirect('path/to/go');
```

**Previous** - To be redirected back to the previous page:

```php
Url::previous();
```

The redirect method can accept a 2nd option of true is used the path will be used as it is provided.

This is useful to redirect to an external website, by default the redirects are relative to the domain its on.

The url should be the local path excluding the application url for instance a valid case might be:

```php
Url::redirect('contacts');
```

The redirect method uses the DIR constant to get the application address.

**template_path has been renamed to templatePath**

The next method is get_templatePath, this returns the path to the template relative from the templates folder, for instance by default it will return: http://www.example.com/templates/default/ this is useful for using absolute paths in your design files such as including css and js files.

```php
Url::templatePath();
```

**autolink has been renamed to autoLink**

Another useful feature is the ability to scan a block of text look for any domain names then convert them into html links. To use the autoLink call url:: followed by the method name and pass in the string to autoLink:

```php
$string = "A random piece of text that contains google.com a domain.";
echo Url::autoLink($string);
```

The autoLink method also accepts a 2nd parameter that will be used as the click text for instance a in the text above I want the link to say Google and not google.com.

```php
$string = "A random piece of text that contains google.com a domain.";
echo Url::autoLink($string, 'Google');
```

When run the link word will be Google which will link to http://google.com
