The database class is used to connect to a MySQL database using the connection details set in the app/Core/Config.php.

The constants (DB_TYPE, DB_HOST, DB_NAME, DB_USER, DB_PASS) are used to connect to the database, the class extends PDO, it can pass the connection details to its parent construct.

```php
try {
    parent::__construct(DB_TYPE.':host='.DB_HOST.'; dbname='.DB_NAME,DB_USER,DB_PASS);
    $this->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
} catch (PDOException $e) {
    Logger::newMessage($e);
    customErrorMsg();
}
```

The error mode is set to use exceptions rather than failing silently in the event of an error. If there is an error they are recorded into the custom log file located in errorlog.html

This class has the following methods:

- <a href='#select'>select</a>
- <a href='#insert'>insert</a>
- <a href='#update'>update</a>
- <a href='#delete'>delete</a>
- <a href='#truncate'>truncate</a>

> NOTE: The methods use prepared statements

<a id='select'></a>
<b>Select</b>
The SELECT query below will return a result set based upon the SQL statement. The result set will return all records from a table called contacts.

```php
$this->db->select("SELECT firstName, lastName FROM ".PREFIX."contacts");
```

Optionally an array can be passed to the query, this is helpful to pass in dynamic values, they will be bound to the query in a prepared statement this ensures the data never goes into the query directly and avoids any possible sql injection.

Example with passed data:

```php
$this->db->select("SELECT firstName, lastName FROM ".PREFIX."contacts WHERE contactID = :id", array(':id' => $id));
```

In this example there is a where condition, instead of passing in an id to the query directly a placeholder is used :id then an array is passed the key in the array matches the placeholder and is bound, so the database will get both the query and the bound data.

<a id='insert'></a>
<b>insert</b>

The insert method is very simple it expects the table name and an array of data to insert:

$this->db->insert(PREFIX.'contacts', $data);
```

The data array is created in a controller, then passed to a method in a model

```php
$postdata = array(
    'firstName' => $firstName,
    'lastName'  => $lastName,
    'email'     => $email
);

$this->model->insertContact($postdata);
```

The model passes the array to the insert method along with the name of the table, optionally return the id of the inserted record back to the controller.

```php
public function insertContact($data)
{
    $this->db->insert(PREFIX.'contacts', $data);
    return $this->db->lastInsertId('contactID');
}
```

<a id='update'></a>
<b>update</b>

The update is very similar to insert an array is passed with data, this time also an identifier is passed and used as the where condition:

Controller:

```php
$postdata = array(
    'firstName' => $firstName,
    'lastName'  => $lastName,
    'email'     => $email
);

$where = array('contactID' => $id);

$this->model->updateContact($postdata, $where);
```

Model:

```php
public function updateContact($data, $where)
{
    $this->db->update(PREFIX.'contacts',$data, $where);
}
```

<a id='delete'></a>
<b>delete</b>

This method expects the name of the table and an array containing the columns and value for the where claus.

Example array to pass:

```php
$data = array('contactID' => $id);
```

Delete in model

```php
public function deleteContact($data)
{
    $this->db->delete(PREFIX.'contacts', $data);
}
```

<a id='truncate'></a>
<b>truncate</b>

This method will delete all rows from the table, the method expects the table name as an argument.

```php
public function deleteContact($table)
{
    $this->db->truncate($table);
}
```
