The document class is a collection of useful methods for working with files.

To get the extension of a file call the getExtension method and pass the file name, the extension will then be returned.

Create an alias

```php
use Nova\Helpers\Document;

Document::getExtension('customfile.zip'); //returns zip
```

To remove the extension of a file call the removeExtension method and pass the file name, the extension will then be removed and returned.

```php
Document::removeExtension('customfile.zip'); //returns customfile
```

To find out the size in a human readable way call the formatBytes method:

```php
Document::formatBytes('4562'); //returns 4.46 KB
```

Using the getFileType a filename is passed and returned is the name of the group that extension belongs to if no matches that 'Other' is returned.

These are the current groups:

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
