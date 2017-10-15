This function connects to google maps and retrieves the lat/lon of the address provided

```php
GeoCode::getLngLat(['Hessle Road', 'Hull'])
```

Returns:

```php
Array
(
    [lon] => -0.348839
    [lat] => 53.7388284
)
```

Parameters are passed as an array there are 4 keys that can be used:

1. Street
2. City
3. State
4. zipcode