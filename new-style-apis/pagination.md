There are several ways to paginate items. The simplest is by using the paginate method on the query builder.

### Paginating Database Results
```php
$users = DB::table('users')->paginate(15);
```

by default the url parameter used to track pages is called offset so use something else such as page add this line to app/Boot/Global.php:

````php
Paginator::setPageName('page');
````

The argument passed to the `paginate` method is the number of items you wish to display per page. Once you have retrieved the results, you may display them on your view, and create the pagination links using the `links` method:

```php
<div class="container">
    <?php foreach ($users as $user): ?>
        <?php echo $user->name; ?>
    <?php endforeach; ?>
</div>

<?php echo $users->links(); ?>
```

You may also access additional pagination information via the following methods:

* getCurrentPage
* getLastPage
* getPerPage
* getTotal
* getFrom
* getTo
* count
* isEmpty()

### "Simple Pagination"

If you are only showing "Next" and "Previous" links in your pagination view, you have the option of using the `simplePaginate` method to perform a more efficient query. This is useful for larger datasets when you do not require the display of exact page numbers on your view:
```php
$someUsers = DB::table('users')->where('votes', '>', 100)->simplePaginate(15);
```

### Creating A Paginator Manually

Sometimes you may wish to create a pagination instance manually, passing it an array of items. You may do so using the `Paginator::make` method:
```php
$paginator = Paginator::make($items, $totalItems, $perPage);
```

### Appending To Pagination Links
You can add to the query string of pagination links using the `appends` method on the Paginator:
```php
<?php echo $users->appends(array('sort' => 'votes'))->links(); ?>
```
This will generate URLs that look something like this:
```php
http://example.com/something?page=2&sort=votes
```
If you wish to append a "hash fragment" to the paginator's URLs, you may use the fragment method:
```php
<?php echo $users->fragment('foo')->links(); ?>
```
This method call will generate URLs that look something like this:
```php
http://example.com/something?page=2#foo
```

### Full Example Usage

**Users Model**
```php
namespace App\Models;

use Database\Model;

class Users extends Model
{
    /**
     * The table associated with the Model.
     *
     * @var string
     */
    protected $table = 'users';

    /**
     * The primary key for the Model.
     *
     * @var string
     */
    protected $primaryKey = 'id';

    /**
     * The number of Records to return for pagination.
     *
     * @var int
     */
    protected $perPage = 25;
}
```

**Users Controller**
```php
namespace App\Controllers;

use Core\View;
use Core\Controller;

class Users extends Controller
{

    private $model;

    public function __construct()
    {
        parent::__construct();

        $this->model = new \App\Models\Users();
    }

    public function dashboard()
    {
        $users = $this->model->paginate();

        return View::make('Users/Dashboard')
            ->shares('title', 'Dashboard')
            ->with('users', $users);
    }
}
```

**Users Dashboard View**

```php
<?php foreach ($users->getItems() as $user): ?>
    <?php echo $user->username; ?>
<?php endforeach ?>

<?php echo $users->links() ?>
```