Table builder helper is a class that would help you to create tables in MySQL (primarily)
without really going into details of SQL query.

# Features<

Table builder allows you to add rows, aliases, set primary key, default values, table name and options.

## Start working with Table Builder

To start working with Table Builder, you need to create instance class of
\Helpers\TableBuilder. Preferably for performance, create your instance class in any model
and pass it \Helpers\Database instance to avoid duplicating database connection.

```
    private $tableBuilder;

    // Declaration of constructor in new model
    public function __construct ()
    {
        parent::__construct();

        // Example of reusing database and avoiding duplicating of database connection
        $this->tableBuilder = new \\Helpers\\TableBuilder($this->db);
    }
```

After initiating new table builder instance you can work with it.

<div class='alert alert-info'>WARNING: Table builder automatically creates a `id` field of type `INT(11)` with `AUTO_INCREMENT` and sets is `PRIMARY KEY`.
If you want to set your own name or don't want to have id field, pass `false` as second parameter.
</div>

<h2>Creating simple table</h2>

Now we can create simple table, let's create table for comments:

```
// Another model's instance
public function createCommentTable ()
{
    // First argument is field name and second is type or alia
    $this->tableBuilder->addField('author', 'VARCHAR(40)');
    $this->tableBuilder->addField('message', 'TEXT');
    $this->tableBuilder->setName('comments');
    $this->tableBuilder->create();
}
```


This example of code would create table named `comments` with `id`, `author` and `message` fields.
If you would try to run this code again you'll see error. To prevent that let's set `IF NOT EXISTS` to true:

```
// First argument is field name and second is type or alia
$this->tableBuilder->addField('author', 'VARCHAR(40)');
$this->tableBuilder->addField('message', 'TEXT');
$this->tableBuilder->setName('comments');
$this->tableBuilder->setNotExists(TRUE);
$this->tableBuilder->create();
```

Now your code shouldn't show any errors.

<h2>Aliases</h2>

Table builder supports aliases instead of using SQL types in `addField` method.
There's only 3 included types: int `INT(11)`, string `VARCHAR(255)` and description `TINYTEXT`.

You can add globally your own alias, for example, in config:

```
// configs above

\Helpers\TableBuilder::setAlias('name', 'VARCHAR(40)');

// configs below

```

<h2>Methods</h2>

<h3>addField</h3>

Method `addField` is used to create field in query:

```
$tableBuilder->addField($field_name, $type_or_alias, $is_null, $options);
```

`field_name` is your name for the field, `type_or_alias` is defined type in MySQL or an alias
defined in tableBuilder, `is_null` is by default is FALSE (so it's not null) but you can
set it to `TRUE` if you needed and options are additional options such as `AUTO_INCREMENT`
or `CURRENT_TIMESTAMP`.

Example of setting field date with `CURRENT_TIMESTAMP`:

```
$tableBuilder->addField('date', 'TIMESTAMP', FALSE, \Helpers\TableBuilder::CURRENT_TIMESTAMP);
```

<h3>setDefault</h3>

Method `setDefault` is used to determine default value of field in query. There's the example:

```
$tableBuilder->setDefault('group_id', 0);
```

This example is illustrating how to set default user `group_id` in table.

> WARNING: Don't use setDefault for timestamps, use `addField` with last argument `\Helpers\TableBuilder::CURRENT_TIMESTAMP` instead.


### create<

Method `create` is used to finish the query and create table in the database:

```
$table->create();
```

You can pass `TRUE` as first argument to reset the tableBuilder and then create another table reusing the same class.

### reset

Method `reset` resets all properties in tableBuilder in order you could start constructing table from beginning. Use it if you need to add construct another table instead of creating new instance of table builder.

## Debugging

If you run into some errors with table builder, you can debug SQL code by calling getSQL method:


```
// Some code ...

echo $this->tableBuilder->getSQL();
```
