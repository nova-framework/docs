# The framework requirements are limited

- PHP 7.1.8 or greater.
- Apache Web Server or equivalent with mod rewrite support.
- For IIS: URL Rewrite module installed - [http://www.iis.net/downloads/microsoft/url-rewrite](http://www.iis.net/downloads/microsoft/url-rewrite)

## The following PHP extensions are required

- Fileinfo (edit php.ini and uncomment php_fileinfo.dll or use php selector within cpanel if available.)
- OpenSSL
- INTL
- MBString

if encountering this error:

**Class 'MessageFormatter' not found**

Then uncomment `extension=php_intl` in the php.ini

> **Note:** Although a database is not required, if a database is to be used, the system is designed to work with a MySQL database using PDO.
