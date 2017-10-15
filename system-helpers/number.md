This helper has 2 methods for converting a number format and to get a percentage.

```php
Number::format($number, $prefix = '4')
```

Converts a given number to start with a prefix, useful to making sure mobile numbers start with a 0 only when they don't start with 0

```php
Number::percentage($val1, $val2)
```

Get a percentage from the 2 given numbers.