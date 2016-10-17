The assets helper is for loading CSS and JS files rather than writing out the full script/link tag for each and every item, instead, add them to an array pass to the assets and it output the link/script tags for you.

>**Note** Template folder names less then 4 characters should be in capital letters ie **Scf** should be **SCF** or resources will not be found. Folders such as Default are OK.

Usage example for loading CSS files:

```php
Assets::css([
    'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
    template_url('css/style.css', 'Default)'
]);
```

Will output:

```php
<link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet" type="text/css">
<link href="http://novaframework.dev/templates/default/assets/css/style.css" rel="stylesheet" type="text/css">
```

Assets have 2 methods CSS and js, they can take a single value or an array of values.

Example of loading a single js file:

```php
Assets::js(template_url('css/app.js', 'Default');
```
