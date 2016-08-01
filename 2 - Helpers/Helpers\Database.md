The database class is used to connect to a MySQL database using the connection details set in the root index.php file.

The constants (DB_TYPE, DB_HOST, DB_NAME, DB_USER, DB_PASS) are used to connect to the database, the class extends PDO, it can pass the connection details to its parent construct.

```php
try {
    parent::__construct(DB_TYPE.':host='.DB_HOST.'; dbname='.DB_NAME,DB_USER,DB_PASS);
    $this->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch(PDOException $e){
    Logger::newMessage($e);
    customErrorMsg();
}
```

The error mode is set to use exceptions rather than failing silently in the event of an error. If there is an error they are recorded into the custom log file located in /errorlog.html

This class has the following methods:

- [select](#select)
- [insert](#insert)
- [update](#update)
- [delete](#delete)
- [truncate](#truncate)

NOTE: The methods use prepared statements

<a id='select'></a>
**Select**
The SELECT query below will return a result set based upon the SQL statement. The result set will return all records from a table called contacts.

```php
$this->_db->select("SELECT * FROM ".PREFIX."contacts");
```

Optionally an array can be passed to the query, this is helpful to pass in dynamic values, they will be bound to the query in a prepared statement this ensures the data never goes into the query directly and avoids any possible sql injection

Example with passed data:
```php
$this->_db->select("SELECT * FROM ".PREFIX."contacts WHERE contactID = :id",
array(':id' => $id));
```

In this example there is a where condition, instead of passing in an id to the query directly a placeholder is used :id then an array is passed the key in the array matches the placeholder and is bound, so the database will get both the query and the bound data.

<a id='insert'></a>
**insert**

The insert method is very simple it expects the table name and an array of data to insert:
```php
$this->_db->insert(PREFIX.'contacts',$data);
```

The data array is created in a controller, then passed to a method in a model

```php
$postdata = array(
    'firstName' => $firstName,
    'lastName'  => $lastName,
    'email'     => $email
);

$this->_model->insert_contact($postdata);
```

The model passes the array to the insert method along with the name of the table, optionally return the id of the inserted record back to the controller.
```php
public function insert_contact($data){
    $this->_db->insert(PREFIX.'contacts',$data);
    return $this->_db->lastInsertId('contactID');
}
```

<a id='update'></a>
**update**

The update is very similar to insert an array is passed with data, this time also an identifier is passed and used as the where condition:

Controller:
```php
$postdata = array(
    'firstName' => $firstName,
    'lastName'  => $lastName,
    'email'     => $email
);

$where = array('contactID' => $id);

$this->_model->update_contact($postdata, $where);
```

Model:
```php
public function update_job($data, $where){
    $this->_db->update(PREFIX.'contacts',$data, $where);
}
```

<a id='delete'></a>
**delete**

This method expects the name of the table and an array containing the columns and value for the where claus.

Example array to pass:
```php
$data = array('contactID' => $id);
```

Delete in model

```php
public function delete_contact($data){
    $this->_db->delete(PREFIX.'contacts', $data);
}
```

<a id='truncate'></a>
**truncate**

This method will delete all rows from the table, the method expects the table name as an argument.

```php
public function delete_contact($table){
    $this->_db->truncate($table);
}
```
