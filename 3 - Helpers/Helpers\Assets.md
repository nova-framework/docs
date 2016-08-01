The assets helper is for loading CSS and JS files rather than writing out the full script/link tag for each and every item, instead, add them to an array pass to the assets and it output the link/script tags for you.

Usage example for loading CSS files:

````php
Assets::css([
    'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
    Url::templatePath().'css/style.css',
]);
```

Will output:

````html
<link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet" type="text/css">
<link href="http://novaframework.dev:8888/templates/default/assets/css/style.css" rel="stylesheet" type="text/css">
```

Assets have 2 methods CSS and js, they can take a single value or an array of values.

Example of loading a single js file:

````php
Assets::js(Url::templatePath().'css/app.js');
```

Both methods have additional parameters other than the file/s:

* **$cache** Default set to false, when set to true the contents will be compressed and compiled into a single file placed within your theme CSS/js folder.

* **$refresh** Default set to false, set to true to update the cache.

* **$cachedMins** Time in seconds to hold the cache.

Example using a cache:

````php
Assets::css([
    'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
    Url::templatePath().'css/style.css',
], true);
```

Example using a cache and refresh:

````php
Assets::css([
    'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
    Url::templatePath().'css/style.css',
], true, true);
```