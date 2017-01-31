# Str

- [Introduction](#introduction)
- [Basic Usage](#basic-usage)
    - [ASCII](#ascii)
    - [Camel](#camel)
    - [Contains](#contains)
    - [Starts With](#starts-with)
    - [Ends With](#ends-with)
    - [Finish](#finish)
    - [Is](#is)
    - [Length](#length)
    - [Limit](#limit)
    - [Lower](#lower)
    - [Upper](#upprt)
    - [Title](#title)
    - [Snake](#snake)
    - [Studly](#studly)
    - [ucfirst](#ucfirst)
    - [Words](#words)
    - [Plural](#plural)
    - [Singular](#singular)
    - [Random Bytes](#random-bytes)
    - [Quick Random](#quick-random)
    - [Equals](#equals)
    - [Slug](#slug)
    - [Substr](#substr)

<a name="introduction"></a>
## Introduction

Str is useful for manipulating string data, you can instantiate it by calling the namespace.

```php
use Str;
```

<a name="basic-usage"></a>
## Basic Usage

<a name="ascii"></a>
### ASCII
Transliterate a UTF-8 value to ASCII:

```php
echo Str::ascii("café äëïöü");
```

Output:

```html
cafe aeiou
```

<a name="camel"></a>
### Camel
Convert a value to camel case:

```php
echo Str::camel("hello world!");
```

Output:

```html
helloWorld!
```

<a name="contains"></a>
### Contains
Determine if a given string contains a given substring, will return a bool:

```php
echo Str::contains("Hello World!", "World");
```

Output:

```html
1
```

<a name="starts-with"></a>
### Starts With
Determine if a given string starts with a given substring, will return a bool:

```php
echo Str::startsWith("Hello World!", "H");
```

Output:

```html
1
```

<a name="ends-with"></a>
### Ends With
Determine if a given string ends with a given substring, will return a bool:

```php
echo Str::endsWith("Hello World!", "!");
```

Output:

```html
1
```

<a name="finish"></a>
### Finish
Cap a string with a single instance of a given value:

```php
echo Str::finish("Hello World!", "111");
```

Output:

```html
Hello World!111
```

<a name="is"></a>
### Is
Cap a string with a single instance of a given value, will return a bool:

```php
echo Str::is("Hello World!", "Hello World!");
```

Output:

```html
1
```

<a name="length"></a>
### Length
Return the length of the given string:

```php
echo Str::length("Hello World!");
```

Output:

```html
12
```

<a name="limit"></a>
### Limit
Limit the number of characters in a string:

```php
echo Str::limit("Hello World!", 5, "[...]");
```

Output:

```html
Hello[...]
```

<a name="lower"></a>
### Lower
Convert the given string to lower-case:

```php
echo Str::lower("Hello World!");
```

Output:

```html
hello world!
```

<a name="upper"></a>
### Upper
Convert the given string to upper-case:

```php
echo Str::upper("Hello World!");
```

Output:

```html
HELLO WORLD!
```

<a name="title"></a>
### Title
Convert the given string to title case:

```php
echo Str::title("hello world!");
```

Output:

```html
Hello World!
```

<a name="snake"></a>
### Snake
Convert the given string to snake case:

```php
echo Str::snake("Hello World!");
```

Output:

```html
hello_world!
```

<a name="studly"></a>
### Studly
Convert the given string to studly case:

```php
echo Str::studly("Hello World!");
```

Output:

```html
HelloWorld!
```

<a name="ucfirst"></a>
### ucfirst
Make a string's first character uppercase:

```php
echo Str::ucfirst("hello world!");
```

Output:

```html
Hello world!
```

<a name="words"></a>
### Words
Limit the number of words in a string:

```php
echo Str::words("Nova Framework is a simple but powerful MVC PHP Framework for building web applications.", 10, "[...]");
```

Output:

```html
Nova Framework is a simple but powerful MVC PHP Framework
```

<a name="plural"></a>
### Plural
Get the plural form of an English word:

```php
echo Str::plural("World");
```

Output:

```html
Worlds
```

<a name="singular"></a>
### Singular
Get the singular form of an English word:

```php
echo Str::singular("Worlds");
```

Output:

```html
World
```

<a name="random-bytes"></a>
### Random Bytes
Generate a more truly "random" bytes:

```php
echo Str::randomBytes(16);
```

Output:

```html
��3!��J�.wLZS*��
```

> OpenSSL must be enabled.

<a name="quick-random"></a>
### Quick Random
Generate a "random" alpha-numeric string.

Should not be considered sufficient for cryptography, etc:

```php
echo Str::quickRandom(16);
```

Output:

```html
lVsrlIVt4hdUf8Nn
```

<a name="equals"></a>
### Equals
Compares two strings using a constant-time algorithm, will return a bool:

```php
echo Str::equals("Nova Framework", "Nova Framework");
```

Output:

```html
1
```

<a name="slug"></a>
### Slug
Generate a URL friendly "slug" from a given string:

```php
echo Str::slug("Nova Framework", "-");
```

Output:

```html
nova-framework
```

<a name="substr"></a>
### Substr
Returns the portion of string specified by the start and length parameters:

```php
echo Str::substr("Nova Framework", 0, 4);
```

Output:

```html
Nova
```