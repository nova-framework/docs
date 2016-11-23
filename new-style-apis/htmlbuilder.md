# HTML Builder

- [Introduction](#introduction)
- [Basic Usage](#basic-usage)
    - [Entities](#entities)
    - [Decode](#decode)
    - [Script](#script)
    - [Style](#style)
    - [Image](#image)
    - [Link](#link)
    - [Secure Link](#secure-link)
    - [Link Asset](#link-asset)
    - [Secure Link Asset](#secure-link-asset)
    - [Link Route](#link-route)
    - [Link Action](#link-action)
    - [Mail To](#mail-to)
    - [Email](#email)
    - [Ordered List](#ordered-list)
    - [Unordered List](#unordered-list)
    - [Obfuscate](#obfuscate)

<a name="introduction"></a>
## Introduction

The html builder contains useful tools where you can create or customise html elements easily. To get started on using the html builder, you can instantiate it by calling the namespace.

```php
use HTML;
```

<a name="basic-usage"></a>
## Basic Usage

<a name="entities"></a>
### Entities
Convert an HTML string to entities:

```php
echo HTML::entities('<i>Hello World!</i>');
```

Output:

```html
&lt;i&gt;Hello World!&lt;/i&gt;
```

<a name="decode"></a>
### Decode
Convert entities to HTML characters:

```php
echo HTML::entities('&lt;i&gt;Hello World!&lt;/i&gt;');
```

Output:

```html
<i>Hello World!</i>
```

<a name="script"></a>
### Script
Generate a link to a JavaScript file:

```php
HTML::script($url, $attributes = array(), false);
```

> Third parameter is for a secure url, setting it to `true` will generate a https url.

```php
echo HTML::script('assets/js/example.js');
```

Output:

```html
<script src="http://www.novaframework.dev/assets/js/example.js"></script>
```

<a name="style"></a>
### Style
Generate a link to a CSS file:

```php
HTML::style($url, $attributes = array(), false);
```

> Third parameter is for a secure url, setting it to `true` will generate a https url.

```php
echo HTML::style('assets/css/example.css');
```

Output:

```html
<link media="all" type="text/css" rel="stylesheet" href="http://www.novaframework.dev/assets/css/example.css">
```

<a name="image"></a>
### Image
Generate an HTML image element:

```php
HTML::image($url, $alt, $attributes = array(), false);
```

> Fourth parameter is for a secure url, setting it to `true` will generate a https url.

```php
echo HTML::image('assets/images/nova.jpg');
```

Output:

```html
<img src="http://www.novaframework.dev/assets/images/nova.jpg">
```

<a name="link"></a>
### Link
Generate a HTML link:

```php
HTML::link($url, $title, $attributes = array(), false);
```

> Fourth parameter is for a secure url, setting it to `true` will generate a https url.

```php
echo HTML::link('subpage', 'Subpage');
```

Output:

```html
<a href="http://www.novaframework.dev/subpage">Subpage</a>
```

<a name="secure-link"></a>
### Secure Link
Generate a HTTPS HTML link:

```php
HTML::secureLink($url, $title, $attributes = array());
```


```php
echo HTML::secureLink('subpage', 'Subpage');
```

Output:

```html
<a href="https://www.novaframework.dev/subpage">Subpage</a>
```

<a name="link-asset"></a>
### Link Asset
Generate a HTML link to an asset:

```php
HTML::linkAsset($url, $title, $attributes = array(), false);
```

<a name="secure-link-asset"></a>
### Secure Link Asset
Generate a HTTPS HTML link to an asset:

```php
HTML::linkRoute($url, $title, $attributes = array());
```

<a name="link-route"></a>
### Link Route
Generate a HTML link to a named route:

```php
HTML::linkRoute($name, $title, $parameters = array(), $attributes = array());
```

<a name="link-action"></a>
### Link Action
Generate a HTML link to a controller action:

```php
HTML::linkAction($action, $title, $parameters = array(), $attributes = array());
```

<a name="mail-to"></a>
### Mail To
Generate a HTML link to an email address:

```php
HTML::mailto($email, $title, $attributes = array());
```

<a name="email"></a>
### Email
Obfuscate an e-mail address to prevent spam-bots from sniffing it:

```php
echo HTML::email('example@novaframework.dev');
```

Output:
```html
&#101;&#120;a&#109;p&#108;&#x65;&#x40;n&#x6f;&#118;&#97;f&#x72;&#x61;&#109;&#101;&#119;or&#x6b;.d&#x65;v
```

<a name="ordered-list"></a>
### Ordered List
Generate an ordered list of items:

```php

$list = [
    'PHP',
    'mySQL',
    'HTML',
    'CSS',
    'Bacon'
];

echo HTML::ol($list, $attributes = array());
```

Output:
```html
<ol><li>PHP</li><li>mySQL</li><li>HTML</li><li>CSS</li><li>Bacon</li></ol>
```

<a name="unordered-list"></a>
### Unordered List
Generate an un-ordered list of items:

```php

$list = [
    'PHP',
    'mySQL',
    'HTML',
    'CSS',
    'Bacon'
];

echo HTML::ul($list, $attributes = array());
```

Output:
```html
<ul><li>PHP</li><li>mySQL</li><li>HTML</li><li>CSS</li><li>Bacon</li></ul>
```

<a name="obfuscate"></a>
### Obfuscate
Obfuscate a string to prevent spam-bots from sniffing it:

```php
$value = 'Test';

echo HTML::obfuscate($value);
```

Output:
```html
&#x54;&#x65#x65;s&#x74;
```