# CSRF Protection

The CSRF helper is used to protect post request from cross site request forgeries. For more information on CSRF see [https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29_Prevention_Cheat_Sheet](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29_Prevention_Cheat_Sheet)

To use place at the top of controller like:<

````
namespace Controllers;

use Core\Controller;    
use Helpers\Csrf;    
use Helpers\Session;

class Pet extends Controller 
{
    private $model;

    public function __construct() 
    {
        parent::__construct();
        $this->model = new \Models\PetModel();
    }
````

In your add or edit method create the token. If you use separate methods to open an edit view and a different method to update, create it in the edit method like:
 
````  
function edit() 
{
    $id = $_GET['id'];
    // or 
    $id = filter_input(INPUT_GET, 'id'); //suggested way....
    $data['csrf_token'] = Csrf::makeToken();
    $data['row'] = $this->model->getPet($id);

    View::renderTemplate('header', $data);
    View::render('pet/edit', $data, $error);
    View::renderTemplate('footer', $data);
}
````

Before the submit button in same view, place this hidden field:
    
````
<input type="hidden" name="token" value="<?php echo $data['csrf_token']; ?>" />
````

In the controller and at the top of the method that processes the form, update here is only an example, place:

````
function update() 
{
    if (isset($_POST['submit'])) { // or the name/value you assign to button.
       if (!Csrf::isTokenValid()) {
            Url::redirect('admin/login'); // Or to a url you choose.......
        }
        
        $id = $_POST['id'];
        $petname = $_POST['petname'];
        // other processing code
````