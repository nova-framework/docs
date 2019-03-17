The language system uses two framework-wide functions for string translation, the first one being **__()**, which is designed to be used in the App folder and the second being **__d()**, designed to be used on Language Domains; those Language Domains are the Modules, Packages and  Themes.

**IMPORTANT:** When a specified domain matches both a Module and Theme name, the Module will take precedence. In other words, there should not be a Module and a Theme with the same name, for the language system to work properly.

The usage of both translation functions is very simple, they contain the string to be translated and optional parameters. For example

```php
// Get a translation for App Domain (default)
$translated = __('Hello, {0}', $username);

$translated = __('Welcome');

// Get a translation for Users Module Domain
$translated = __d('users', 'Welcome back, {0}!', $username);

$translated = __d('users', 'Welcome back!');
```

Please note that the Domains are specified in **snake_case** transformation and that the translation string's parameters are using the ICU notation, which is used also by the PHP's INTL extension.

The language files used by this language system are called **messages.php** and will be found in the locations, i.e. **app/Language/EN/messages.php**
