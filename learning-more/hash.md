The Hash service using php 5 password_ functions.

To create a hash of a password, call the make method and provide the password to be hashed, once done save the $hash.

```php
$hash = Hash::make($password);
```

When logging in a user their hash must be retrieved from the database and compared against the provided password to make sure they match, for this a method called password_verify is used, it has 2 parameters the first is the user provided password the second is the hash from the database.

```php
if (Hash::check($_POST['password'], $password)) {
     //passed
} else {
     //failed
}
```

From time to time you may update your hashing parameters (algorithm, cost, etc). So a function to determine if rehashing is necessary is available:

```php
if (Hash::check($password, $hash)) {     
   if (Hash::needsRehash($hash, $algorithm, $options)) {         
       $hash = Hash::make($password, $algorithm, $options); /* Store new hash in db */     
   } 
}
```
