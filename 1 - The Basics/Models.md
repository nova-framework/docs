Models control the data source, they are used for collecting and issuing data, this could be from a remote service as XML,JSON or using a database to get and fetch records.

A Model is structured like a controller; it's a class. The model needs to extend the parent Model. Like a controller the constuctor needs to call the parent contstruct in order to gain access to its properties and methods.

```php
class Contacts_Model extends Model {

    function __construct(){
        parent::__construct();
    }

}
```

The parent model is very simple it's only role is to create an instance of the database class located in (app/helpers/database.php) once set the instance is available to all child models that extend the parent model.

```php
class Model {

    protected $_db;

    function __construct(){
        $this->_db = new Database();
    }
}
```

> Models can be placed in the root of the models folder it in sub-folders, the convention for the filenames are name of the model plus _model for a welcome model becomes welcome_model.php

Methods inside a model are used for getting data and returning data back to the controller, a method should never echo data only return it, it's the controller that decides what is done with the data once it's returned.

The most common us of a model is for performing database actions, here is a quick example:

```php
public function getContacts(){
    return $this->_db->select('SELECT firstName,lastName FROM '.PREFIX.'contacts');
}
```

This is a very simple database query $this->_db is available from the parent model inside the $_db class holds methods for selecting,inserting,updating and deleting records from a MySQL database using PDO more on this topic in the **Database** section.
