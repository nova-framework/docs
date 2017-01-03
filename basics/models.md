Models control the data source, they are used for collecting and issuing data, this could be from a remote service as XML,JSON or using a database to get and fetch records.

A Model is structured like a controller; it's a class. The model needs to extend the parent Model. Like a controller the constuctor needs to call the parent construct in order to gain access to its properties and methods.

```php
<?php
namespace Models;

use Core\Model;

class Contacts extends Model
{
    function __construct(){
        parent::__construct();
    }
}
```

The parent model is very simple it's only role is to create an instance of the database class located in (app/Helpers/Database.php) once set the instance is available to all child models that extend the parent model.

```php
<?php
namespace Core;

use Helpers\Database;

class Model
{
    protected $db;

    public function __construct()
    {
        //connect to PDO here.
        $this->db = Database::get();

    }
}
```

Models can be placed in the root of the models folder. The namespace used in the model should reflect its file path. Classes directly in the models folder will have a namespace of models or if in a folder: namespace \\Models\\Classname;

Methods inside a model are used for getting data and returning data back to the controller, a method should never echo data only return it, it's the controller that decides what is done with the data once it's returned.

The most common us of a model is for performing database actions, here is a quick example:

```php
public function getContacts()
{
    return $this->db->select('SELECT firstName,lastName FROM '.PREFIX.'contacts');
}
```

This is a very simple database query $this->db is available from the parent model inside the $db class holds methods for selecting, inserting, updating and deleting records from a MySQL database using PDO more on this topic in the section <a href='http://novaframework.com/documentation/v2.2/database'>Database</a>.
