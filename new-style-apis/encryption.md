- [Configuration](#configuration)
- [Basic Usage](#basic-usage)
    - [Encrypting a value](#encrypting-a-value)
    - [Decrypting a value](#decrypting-a-value)

## Configuration

Before using Nova's encryptor, you should set the `ENCRYPT_KEY` option of your app/Config.php configuration file to a 32 character, random string. If this value is not properly set, all values encrypted by Nova will be insecure.

For an easy way to set an encryption key, navigate to your project directory and use `php nova make:key` in your console/terminal.

## Basic Usage

#### Encrypting a value

You may encrypt a value using the `Crypt` facade. All encrypted values are encrypted using `OpenSSL` and the `AES-256-CBC` cipher. Furthermore, all encrypted values are signed with a message authentication code (MAC) to detect any modifications to the encrypted string.

```php
use Crypt;

Crypt::encrypt($secret);
```

#### Decrypting a value

Of course, you may decrypt values using the `decrypt` method on the `Crypt` facade. If the value can not be properly decrypted, such as when the MAC is invalid, an Encryption\DecryptException will be thrown:

```php
use Crypt;
use Encryption\DecryptException;

try {
    $decrypted = Crypt::decrypt($encryptedValue);
} catch (DecryptException $e) {
    //
}
```