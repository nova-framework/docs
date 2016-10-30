There are several ways to paginate items. The simplest is by using the paginate method on the query builder.

### Paginating Database Results
```php
//pass the number of records to limit per page, the default is 25
$users = DB::table('users')->paginate(15);
```

By default the url parameter used to track pages is called offset so use something else such as page add this line to app/Boot/Global.php:

```php
Paginator::setPageName('page');
```

The argument passed to the `paginate` method is the number of items you wish to display per page. Once you have retrieved the results, you may display them on your view, and create the pagination links using the `links` method:

```php
<div class="container">
    <?php
    foreach ($users as $user) {
        echo $user->name;
    }
    ?>
</div>
```
Display the page links

```php
<?=$users->links();?>
```

You may also access additional pagination information via the following methods:

* getCurrentPage
* getLastPage
* getPerPage
* getTotal
* getFrom
* getTo
* count
* isEmpty

### "Simple Pagination"

If you are only showing "Next" and "Previous" links in your pagination view, you have the option of using the `simplePaginate` method to perform a more efficient query. This is useful for larger datasets when you do not require the display of exact page numbers on your view:

```php
$someUsers = DB::table('users')->where('votes', '>', 100)->simplePaginate(15);
```

### Creating A Paginator instance Manually

Sometimes you may wish to create a pagination instance manually, passing it an array of items. You may do so using the `Paginator::make` method:

```php
use Paginator;

$paginator = Paginator::make($items, $totalItems, $perPage);
```

### Appending To Pagination Links
You can add to the query string of pagination links using the `appends` method on the Paginator:

```php
$users->appends(array('sort' => 'votes'))->links();
```
This will generate URLs that look something like this:

```php
http://example.com/something?page=2&sort=votes
```
If you wish to append a "hash fragment" to the paginator's URLs, you may use the fragment method:

```php
$users->fragment('aomething')->links();
```
This method call will generate URLs that look something like this:

```php
http://example.com/something?page=2#foo
```

### Full Example Usage

**Users Model**

```php
namespace App\Models;

use Nova\Database\ORM\Model as BaseModel;

class User extends BaseModel
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

use App\Core\View;
use App\Core\Controller;

use App\Models\User;

class Users extends Controller
{
    public function __construct()
    {
        parent::__construct();
    }

    public function dashboard()
    {
        $users = User::paginate(25);

        return View::getView()
            ->shares('title', 'Dashboard')
            ->with('users', $users);
    }
}
```

**Users Dashboard View**

```php
foreach ($users->getItems() as $user) {
    echo $user->username;
}

echo $users->links();¡
?>
```