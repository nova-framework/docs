- [Configuration](#configuration)
- [Session Usage](#session-usage)
- [Flash Data](#flash-data)
- [Session Drivers](#session-drivers)

## Configuration

Since HTTP driven applications are stateless, sessions provide a way to store information about the user across requests. Nova ships with a variety of session back-ends available for use through a clean, unified API.

The session configuration is stored in **app/Config/Session.php**. Be sure to review the well documented options available to you in this file. By default, Nova is configured to use the file session driver, which will work well for the majority of applications.

#### Reserved Keys

The Nova framework uses the `flash` session key internally, so you should not add an item to the session by that name.

## Session Usage

#### Storing An Item In The Session

```php
Session::put('key', 'value');
```

#### Push A Value Onto An Array Session Value

```php
Session::push('user.teams', 'developers');
```

#### Retrieving An Item From The Session

```php
$value = Session::get('key');
```

#### Retrieving An Item Or Returning A Default Value

```php
$value = Session::get('key', 'default');

$value = Session::get('key', function() { return 'default'; });
```

#### Retrieving An Item And Forgetting It

```php
$value = Session::remove('key', 'default');
```

#### Retrieving All Data From The Session

```php
$data = Session::all();
```

#### Determining If An Item Exists In The Session

```php
if (Session::has('users'))
{
    //
}
```

#### Removing An Item From The Session

```php
Session::forget('key');
```

#### Removing All Items From The Session

```php
Session::flush();
```

#### Regenerating The Session ID

```php
Session::regenerate();
```

#### Getting a users intended url before logging into a system

Inside the Session data is the following:

```
[url] => Array
(
   [intended] => http://noveframework.dev/usefullinks/create
)
```

This is the a link of the url attempted before the authentication happened, causing the use to have to login before getting to the page. This can be used like this: (array keys can be accessed using a dot notation ie url.intended)

```php
//if the session key exists use it or set a url to use.
if (Session::get('url.intended')) {
    $url = Session::get('url.intended');
} else {
    $url = 'dashboard';
}

// Redirect to url
return Redirect::to($url);
```

Or use Redirect::intended() which will do the same as above automatically.

```php
return Redirect::intended('dashboard');
````

## Flash Data

Sometimes you may wish to store items in the session only for the next request. You may do so using the `Session::flash` method:

```php
Session::flash('key', 'value');
```

#### Reflashing The Current Flash Data For Another Request

```php
Session::reflash();
```

#### Reflashing Only A Subset Of Flash Data

```php
Session::keep(array('username', 'email'));
```

## Session Drivers

The session "driver" defines where session data will be stored for each request. Nova ships with several great drivers out of the box:

- `file` - sessions will be stored in app/Storage/Sessions.
- `database` - sessions will be stored in a database used by your application.
