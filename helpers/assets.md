The assets helper is for loading CSS and JS files rather than writing out the full script/link tag for each and every item, instead, add them to an array pass to the assets and it output the link/script tags for you.

>**Note** Theme folder names less then 4 characters should be in capital letters ie **Scf** should be **SCF** or resources will not be found.

Usage example for loading CSS files:

```php
Assets::css([
    'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
    theme_url('css/style.css', 'Bootstrap)'
]);
```

Will output:

```php
<link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet" type="text/css">
<link href="http://novaframework.dev/templates/bootstrap/assets/css/style.css" rel="stylesheet" type="text/css">
```

Assets have 2 methods CSS and js, they can take 2 params: 
1. a single value or an array of values.
2. true or false to return the tag. False is set by default causing the tag to be echo'd instead of returned

Example of loading a single js file:

```php
Assets::js(theme_url('css/app.js', 'Bootstrap');
```
