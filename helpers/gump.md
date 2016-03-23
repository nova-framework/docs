#GUMP

GUMP is a fast, extensible & stand-alone PHP input validation class that allows you to validate any data.

To use the class call it by using the namespace Helpers\Gump

An example validating a username and password:

````
$is_valid = Gump::is_valid($_POST, array(
    'username' => 'required|alpha_numeric',
    'password' => 'required|max_len,100|min_len,6'
));

if ($is_valid === true) {
    // continue
} else {
    print_r($is_valid);
}
````

To pass the error array to the view pass $is_valid (this will hold any errors) as a third param to the render method.

To loop through and display errors without doing it yourself call the display method of the error class:

````
echo Error::display($error); 
````

For further examples see [https://github.com/Wixel/GUMP](https://github.com/Wixel/GUMP)