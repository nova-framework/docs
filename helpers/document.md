- [Introduction](#introduction)
- [Setup](#setup)
- [Methods](#methods)
    - [getFileType](#getFileType)
    - [formatBytes](#formatBytes)
    - [getBytesSize](#getBytesSize)
    - [getFolderSize](#getFolderSize)
    - [getExtension](#getExtension)
    - [removeExtension](#removeExtension)

<a name="introduction"></a>
## Introduction

The `Document` helper class is a collection of useful methods for working with files.

<a name="setup"></a>
## Setup

To use this helper, you can call the following alias or namespace:

```php
use Document;

// OR

use Nova\Helpers\Document;
```

<a name="methods"></a>
## Methods

<a name="getFileType"></a>
### getFileType
To get the type of a file call the `getFileType` method asnd pass the filename, the file type will then be returned. If there are no matches, the file type 'Other' is returned.

These are the current file type groups:

```php
$images = array('jpg', 'gif', 'png', 'bmp');
$docs = array('txt', 'rtf', 'doc', 'docx', 'pdf');
$apps = array('zip', 'rar', 'exe', 'html');
$video = array('mpg', 'wmv', 'avi', 'mp4');
$audio = array('wav', 'mp3');
$db = array('sql', 'csv', 'xls','xlsx');
```

```php
Document::getFileType('customfile.zip'); //returns Application
```

<a name="formatBytes"></a>
### formatBytes
To find out the size in a human readable way call the `formatBytes` method:

```php
Document::formatBytes('4562'); //returns 4.46 KB
```

<a name="getByteSize"></a>
### getByteSize

To convert a human readable file size to a number of bytes that it represents, call the `getByteSize` method.

```php
Document::getByteSize('10K'); //returns 10240
```

> Supports the following modifiers: K, M, G and T.
> Invalid input is returned unchanged.

<a name="getFolderSize"></a>
### getFolderSize
To get the byte size of a folder, pass a path to the `getFolderSize` method.

```php
Document::getFolderSize('path/to/folder'); //returns size in bytes.
```

<a name="getExtension"></a>
### getExtension
To get the extension of a file call the `getExtension` method and pass the file name, the extension will then be returned.

```php
Document::getExtension('customfile.zip'); //returns zip
```

<a name="removeExtension"></a>
### removeExtension
To remove the extension of a file call the `removeExtension` method and pass the file name, the extension will then be removed and returned.

```php
Document::removeExtension('customfile.zip'); //returns customfile
```


