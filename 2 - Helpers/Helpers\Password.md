The password file uses a password library from <a href='https://github.com/ircmaxell/password_compat'>https://github.com/ircmaxell/password_compat</a>

The  library is intended to provide forward compatibility with the <a href="http://php.net/password">password_*</a> functions being worked on for PHP 5.5<

This library requires PHP >= 5.3.7 OR a version that has the $2y fix backported into it (such as RedHat provides). Note that Debian's 5.3.3 version is <strong>NOT</strong> supported.

To create a hash of a password, call the make method and provide the password to be hashed, once done save the $hash.

```
$hash = \\Helpers\\Password::make($password);
```

When logging in a user their hash must be retrieved from the database and compared against the provided password to make sure they match, for this a method called password_verify is used, it has 2 parameters the first is the user provided password the second is the hash from the database.

```
    if (\\Helpers\\Password::verify($_POST['password'], $data[0]->password)) {
     //passed
    } else {
     //failed
    }
```

From time to time you may update your hashing parameters (algorithm, cost, etc). So a function to determine if rehashing is necessary is available:

```
if (\\Helpers\\Password::verify($password, $hash)) {
   if (\\Helpers\\Password::needsRehash($hash, $algorithm, $options)) {
    $hash = \\Helpers\\Password::make($password, $algorithm, $options); /* Store new hash in db */
   }
}
```
