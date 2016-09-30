- [Introduction](#introduction)
- [Configuration](#configuration)
- [Storing Passwords](#storing-passwords)
- [Authenticating Users](#authenticating-users)
- [Basic Usage](#basic-usage)

## Introduction

All the classes of the **Auth** system live in the namespace **Auth** and is implemented as a reference structure for user authentication in the **App** namespace.

To note that additional Route Filters are also added to support this reference implementation, and the proper configuration of a valid `ENCRYPT_KEY` is required.

Being users management, a database is required and in **scripts/nova_users.sql** you will find the associated MySQL dump for a users table.

The **App\Modules\Users** also implements a small private area for the authenticated user. The private area is a simple dashboard and a profile page, where the users have the ability to change their password.

> **Important:** Nova's authentication uses the new Database API and not the Helpers\Database. If you choose to use the Nova authentication, you would need to use the new database API in the whole application and to not touch the helpers\database instances.

## Configuration

Nova aims to make implementing authentication very simple. In fact, almost everything is configured for you out of the box. The authentication configuration file is located at **app/Config/Auth.php**, which contains several well documented options for tweaking the behaviour of the authentication facilities.

By default, Nova includes a u¡ser model in your **app/Models** directory which may be used with the default `extended` authentication driver, which uses `Database\ORM`.

If your application is not using `ORM`, you may use the `database` authentication driver which uses the Nova query builder.

## Storing Passwords

The Nova `Hash` class provides secure `Bcrypt` hashing:

#### Hashing A Password Using Bcrypt
```php
$password = Hash::make('secret');
```

#### Verifying A Password Against A Hash
```php
if (Hash::check('secret', $hashedPassword))
{
    // The passwords match...
}
```

#### Checking If A Password Needs To Be Rehashed
```php
if (Hash::needsRehash($hashed))
{
    $hashed = Hash::make($hashed);
}
```

## Authenticating Users

To log a user into your application, you may use the `Auth::attempt` method.

```php
if (Auth::attempt(array('email' => $email, 'password' => $password)))
{
    // User is authenticated there.
}
```

Take note that `email` is not a required option, it is merely used for an example. You should use whatever column name corresponds to a "username" in your database. The `Redirect::intended` function will redirect the user to the URL they were trying to access before being caught by the authentication filter. A fallback URI may be given to this method in case the intended destination is not available.

When the attempt method is called, the `auth.attempt` event will be fired. If the authentication attempt is successful and the user is logged in, the `auth.login` event will be fired as well.

#### Determining If A User Is Authenticated

To determine if the user is already logged into your application, you may use the `check` method:

```php
if (Auth::check())
{
    // The user is logged in...
}
```

#### Authenticating A User And "Remembering" Them

If you would like to provide "remember me" functionality in your application, you may pass `true` as the second argument to the `attempt` method, which will keep the user authenticated indefinitely (or until they manually logout). Of course, your users table must include the string `remember_token` column, which will be used to store the "remember me" token.

```php
if (Auth::attempt(array('email' => $email, 'password' => $password), true))
{
    // The user is being remembered...
}
```
> Note: If the attempt method returns true, the user is considered logged into the application.

#### Determining If User Authenticated Via Remember

If you are "remembering" user logins, you may use the `viaRemember` method to determine if the user was authenticated using the "remember me" cookie:

```php
if (Auth::viaRemember())
{
    //
}
```

#### Authenticating A User With Conditions

You also may add extra conditions to the authenticating query:

```php
if (Auth::attempt(array('email' => $email, 'password' => $password, 'active' => 1)))
{
    // The user is active, not suspended, and exists.
}
```

> Note: For added protection against session fixation, the user's session ID will automatically be regenerated after authenticating.

#### Accessing The Logged In User

Once a user is authenticated, you may access the User model / record:

```php
$email = Auth::user()->email;
```

To retrieve the authenticated user's ID, you may use the `id` method:

```php
$id = Auth::id();
```

To simply log a user into the application by their ID, use the `loginUsingId` method:

```php
Auth::loginUsingId(1);
```

#### Validating User Credentials Without Login

The `validate` method allows you to validate a user's credentials without actually logging them into the application:

```php
if (Auth::validate($credentials))
{
    //
}
```

#### Logging A User In For A Single Request

You may also use the `once` method to log a user into the application for a single request. No sessions or cookies will be utilised.

```php
if (Auth::once($credentials))
{
    //
}
```

#### Logging A User Out Of The Application
```php
Auth::logout();
```

## Basic Usage

```php
    public function postLogin()
    {
        // Retrieve the Authentication credentials.
        $credentials = Input::only('username', 'password');

        // Prepare the 'remember' parameter.
        $remember = (Input::get('remember') == 'on');

        // Make an attempt to login the Guest with the given credentials.
        if(! Auth::attempt($credentials, $remember)) {
            // An error has happened on authentication.
            $status = __d('users', 'Wrong username or password.');

            return Redirect::back()->withStatus($status, 'danger');
        }

        // The User is authenticated now; retrieve his Model instance.
        $user = Auth::user();

        if (Hash::needsRehash($user->password)) {
            $password = $credentials['password'];

            $user->password = Hash::make($password);

            // Save the User Model instance - used with the Extended Auth Driver.
            $user->save();

            // Save the User Model instance - used with the Database Auth Driver.
            //$this->model->updateGenericUser($user);
        }

        if($user->active == 0) {
            Auth::logout();

            // User not activated; logout and redirect him back.
            $status = __d('users', 'There is a problem. Have you activated your Account?');

            return Redirect::back()->withStatus($status, 'warning');
        }

        // Prepare the flash message.
        $status = __d('users', '<b>{0}</b>, you have successfully logged in.', $user->username);

        // Redirect to the User's Dashboard.
        return Redirect::to('admin/dashboard')->withStatus($status);
    }
```