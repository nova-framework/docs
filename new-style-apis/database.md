- [Introduction](#introduction)
- [Basic Usage](#basic-usage)
- [Selects](#selects)
- [Joins](#joins)
- [Aggregates](#aggregates)
- [Raw Expressions](#raw-expressions)
- [Inserts](#inserts)
- [Updates](#updates)
- [Deletes](#deletes)
- [Unions](#unions)

## Introduction
An improved Database API was recently added, which includes a **QueryBuilder** and a simple but powerful Model. Everything regarding this API is living within the namespace **'\Database\'** for isolation reasons.

To note that this new Database API doesn't replace any of the existing classes, the actual **Core\Model** and **Helpers\Database** remain untouched. The end-user can choose which Database API is used in their application, with the only condition to not use both of them simultaneously, which will duplicate the Database connections.

## Basic Usage

Via a Facade which permit commands like:

```php
use DB;

$prefix = DB::getTablePrefix();
$data = DB::select("SELECT * FROM {$prefix}users");
```

## Multiple connections

Connect to a different database by passing the connection name to connection() the connections are setup in **app/Config/Database.php**

```php
DB::connection('mysql')->table('users')->get();
```

Commands for **insert/update/delete** are available.

QueryBuilder has support for commands like:

```php
use DB;

$users = DB::table('users')->get();

$users = DB::table('users')->where('role', '=', 'admin')->get();
```

Alternatively a Model can be used, which uses the Database\Model, instead of Helpers\Database.

```php
namespace App\Models;

use Database\Model;

class Users extends Model
{
    protected $table = 'users';
    protected $primaryKey = 'id';
}
```

To note the two protected variables, which specify the table name and Primary Key, which are mandatory to be configured, especially the Table name, the second one defaulting to 'id'.

A **Database\Model** is similar as usage with the **Core\Model**, but have the ability to transparently use the QueryBuilder methods, then permitting command like this:

```php
use App\Models\Users;

$model = new Users();

$users = $model->where('role', '=', 'admin')->get();
```

Also, the **Database\Model** offer associated **QueryBuilder** instances, as following:

```php
use App\Models\Users;

$model = new Users();

$query = $model->newQuery();

$users = $query->where('role', '=', 'admin')->get();
```

## Selects

#### Retrieving All Rows From A Table
```php
$users = DB::table('users')->get();

foreach ($users as $user)
{
    var_dump($user->name);
}
```

#### Retrieving A Single Row From A Table
```php
$user = DB::table('users')->where('name', 'John')->first();

var_dump($user->name);
```

#### Retrieving A Single Column From A Row
```php
$name = DB::table('users')->where('name', 'John')->pluck('name');
```

#### Retrieving A List Of Column Values
```php
$roles = DB::table('roles')->lists('title');
```
This method will return an array of role titles. You may also specify a custom key column for the returned array:

```php
$roles = DB::table('roles')->lists('title', 'name');
```

#### Specifying A Select Clause
```php
$users = DB::table('users')->select('name', 'email')->get();

$users = DB::table('users')->distinct()->get();

$users = DB::table('users')->select('name as user_name')->get();
```

#### Using Where Operators
```php
$users = DB::table('users')->where('votes', '>', 100)->get();
```

#### Or Statements
```php
$users = DB::table('users')
    ->where('votes', '>', 100)
    ->orWhere('name', 'John')
    ->get();
```

#### Using Where Between
```php
$users = DB::table('users')
    ->whereBetween('votes', array(1, 100))->get();
```

#### Using Where Not Between
```php
$users = DB::table('users')
    ->whereNotBetween('votes', array(1, 100))->get();
```

#### Using Where In With An Array
```php
$users = DB::table('users')
    ->whereIn('id', array(1, 2, 3))->get();

$users = DB::table('users')
    ->whereNotIn('id', array(1, 2, 3))->get();
```

#### Using Where Null To Find Records With Unset Values
```php
$users = DB::table('users')
    ->whereNull('updated_at')->get();
```

#### Order By, Group By, And Having
```php
$users = DB::table('users')
    ->orderBy('name', 'desc')
    ->groupBy('count')
    ->having('count', '>', 100)
    ->get();
```

#### Offset & Limit
```php
$users = DB::table('users')->skip(10)->take(5)->get();
```

## Joins

The query builder may also be used to write join statements. Take a look at the following examples:

#### Basic Join Statement
```php
DB::table('users')
    ->join('contacts', 'users.id', '=', 'contacts.user_id')
    ->join('orders', 'users.id', '=', 'orders.user_id')
    ->select('users.id', 'contacts.phone', 'orders.price')
    ->get();
```

#### Left Join Statement
```php
DB::table('users')
    ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
    ->get();
```

## Aggregates

The query builder also provides a variety of aggregate methods, such as `count`, `max`, `min`, `avg`, and `sum`.

#### Using Aggregate Methods
```php
$users = DB::table('users')->count();

$price = DB::table('orders')->max('price');

$price = DB::table('orders')->min('price');

$price = DB::table('orders')->avg('price');

$total = DB::table('users')->sum('votes');
```

## Raw Expressions

Sometimes you may need to use a raw expression in a query. These expressions will be injected into the query as strings, so be careful not to create any SQL injection points! To create a raw expression, you may use the `DB::raw` method:

#### Using A Raw Expression
```php
$users = DB::table('users')
    ->select(DB::raw('count(*) as user_count, status'))
    ->where('status', '<>', 1)
    ->groupBy('status')
    ->get();
```

## Inserts

### Inserting Records Into A Table
```php
DB::table('users')->insert(
    array('email' => 'john@example.com', 'votes' => 0)
);
```

#### Inserting Records Into A Table With An Auto-Incrementing ID
If the table has an auto-incrementing id, use `insertGetId` to insert a record and retrieve the id:

```php
$id = DB::table('users')->insertGetId(
    array('email' => 'john@example.com', 'votes' => 0)
);
```
**Note:** When using PostgreSQL the `insertGetId` method expects the auto-incrementing column to be named "id".

#### Inserting Multiple Records Into A Table
```php
DB::table('users')->insert(array(
    array('email' => 'daniel@example.com', 'votes' => 0),
    array('email' => 'taylor@example.com', 'votes' => 0),
));
```

## Updates

#### Updating Records In A Table
```php
DB::table('users')
    ->where('id', 1)
    ->update(array('votes' => 1));
```

#### Incrementing or decrementing a value of a column
```php
DB::table('users')->increment('votes');

DB::table('users')->increment('votes', 5);

DB::table('users')->decrement('votes');

DB::table('users')->decrement('votes', 5);
```
You may also specify additional columns to update:

```php
DB::table('users')->increment('votes', 1, array('name' => 'John'));
```

## Deletes

#### Deleting Records In A Table
```php
DB::table('users')->where('votes', '<', 100)->delete();
```

#### Deleting All Records From A Table
```php
DB::table('users')->delete();
```

#### Truncating A Table
```php
DB::table('users')->truncate();
```

## Unions

The query builder also provides a quick way to "union" two queries together:

```php
$first = DB::table('users')->whereNull('first_name');

$users = DB::table('users')->whereNull('last_name')->union($first)->get();
```
The `unionAll` method is also available, and has the same method signature as `union`.