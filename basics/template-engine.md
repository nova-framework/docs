# In 3.76.0 a new template engine was added.

The native **Template Engine** responds to View files with the extension `.tpl`all views which will use the template syntax 
should be have the extension .tpl view files with .php won't be able to use the template engine.

## Control Structure

### Printing variables 

To display a variable it should be wrapped inside a set of `{{` and to close `}}`

```php
{{ $title }}
```

Variables wrapped inside {{ }} are **Note** escaped on output. All data user generated should alawys be escaped. 
When using user generated variables use:

```php
{{{ $title }}}
```

Using 3 curly braces **Will** escape and is safer to use.

Variables passed inside the braces will be executed no need to open php and close it after, this keeps the view files 
clean and flexible.

### Printing a variable or using a default

If a variable might not exist you can echo it and provide a default value:

```php
{{ $name OR 'no name provided' }}
```

This will result in $name being used if it exists otherwise the string 'no name provided' would be printed.
