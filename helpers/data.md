Data helper contains a bunch of useful methods for looking at and altering your data.
````php
Data::pr($data)
```
Returns the data inside a print_r wrapped around pre tags.
````php
Data::vd($data)
```
Returns var_dump
````php
Data::sl($data)
```
Returns the length of the string
````php
Data::stu($data)
```
Convert to uppercase
````php
Data::stl($data)
```
Convert to lower
````php
Data::ucw($data)
```
Returns the string with the first letter of each word in uppercase using ucwords
````php
Data::createKey($length = 32)
```
Create a key and set the length.