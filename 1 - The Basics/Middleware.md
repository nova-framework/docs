The middleware gives a single shot to the views associated into Controllers, before executing the requested Method, and store the resulted data in a Views shared storage, from where its loaded and added for rendering execution. This style avoids multiple calling of those Hooks, as in for every rendering call.

In the **Core\View** there is a Method, called "share", permitting to set data variables which are shared by any called rendering method. For example, it is possible to have:

````php
return View::share('title', 'Page Title');
```

Making the variable **'title'** available in any rendering command. 

When a request is initiated a "before" method is automatically executed, it by default executes the Views associated Hooks and share their result to Core\View. This allows to fine tune the Controller behavior, for example hosting logic to check the user authentication or do whatever pre-processing, i.e. checking access codes, without requiring to add commands to every method, simplifying the code.

A complex example of its chained usage can be:

````php
// Into a Base Controller

protected function before()
{
    // Check if the User is authorized to use the requested Method.
    switch ($this->getMethod()) {
        case 'login':
        case 'forgot':
        case 'reset':
            break;
        default:
            if (Session::get('loggedIn') == false) {
                Url::redirect('login');
            }
    };

    // Leave to parent's Method the execution decisions.
    return parent::before(); // <-- there are loaded the Views associated Hooks.
}
// Into an Admin Controller

protected function before()
{
    // Check the CSRF token for select Methods over POST, e.g: add/edit/delete.
    switch($this->getMethod()) {
        case 'index':
        case 'show':
            break;
        default:
            if (Request::isPost() && ! Csrf::isTokenValid()) {
                Url::redirect('login');
            }
    };

    // Leave to parent's Method the execution decisions.
    return parent::before(); // <-- there is checked the User Authorization.
}
```

Another more complex example, for a Controller for private files serving; with access authorization and limit the access time:

````php
protected function before()
{
    // Only the authorized Users can arrive to the Methods from this Controller.
    if (Session::get('loggedIn') == false) {
        Url::redirect('login');
    }

    //  
    if($this->getMethod() == 'files') {
        $params = $this->params();

        if(count($params) != 3) {
             header("HTTP/1.0 400 Bad Request");

             return false;
        }

        // The current timestamp.
        $timestamp = time();

        // File access validation elements.
        $validation = $params[0];
        $time       = $params[1];
        $filename   = $params[2];

        $clientip = $this->getIpAddress();

        $hash = hash('sha256', $clientip.$filename.$time.FILES_ACCESSKEY);

        if((($timestamp - $time) > FILES_VALIDITY) || ($validation != $hash)) {
            header("HTTP/1.0 403 Forbidden");

            return false;  
        }
    }  else if (Request::isPost()) {
         // All the POST requests should have a CSRF Token there.
         if (! Csrf::isTokenValid()) {
             Url::redirect('login');
         }
    }

    // Leave to parent's Method the execution decisions.
    return parent::before(); // <-- there are loaded the Views associated Hooks.
}
```

Also, the part of Method flow execution was moved into **Core\Controller::execute()**, presenting the big advantage to simplify the Routing and permitting to have the **before/after** methods protected, safe against their execution from outside.

Finally, into **Core\Controller** was introduced an new method called "**trans**", wrapping up the Language translation. Then, permitting commands like:

````php
// Actual style
$data['title'] = $this->language->get('welcomeText');
$data['welcomeMessage'] = $this->language->get('welcomeMessage');

// New style
$data['title'] = $this->trans('welcomeText');
$data['welcomeMessage'] = $this->trans('welcomeMessage');
```

Example of using "**View::share()**"

````php
public function index()
{
    $title = $this->trans('welcomeText');

    $data['welcomeMessage'] = $this->trans('welcomeMessage');

    // Make $title available in all Views rendering
    View::share('title', $title);

    View::renderTemplate('header'); // <-- No need for $data in this case
    View::render('Welcome/Welcome', $data);
    View::renderTemplate('footer'); // <-- No need for $data in this case
}
```

**What is difference between applying pre-processing in "__constructor()" and "before()" ?**

Comparative with running checks in Class Constructor, the **before()** method is executed WHEN the requested Method is a valid one, both as in existing and being "callable", and it have at its disposition the requested Method name and the associated Parameters; both passed to by Routing. Then, **before()** can accurate tune the Controller behavior, depending on requested Method.

**Is required to implement the methods "before()" and "after()" in every Controller ?**

No. You can safely completely ignore their existence if you don't want to fine tune the Controller's behavior via that Middleware.

**What if I want a base Controller which do NOT call the Views associated Hooks ?**

Just override the **before()** like below in a Base Controller, e.g. **App\Core\Controller**

````php
namespace App\Core;

use Core\Controller as BaseController

class Controller extends BaseController
{
    public function __construct()
    {
        parent::__construct();
    }

    protected function before()
    {
        return true;
    }
}
```

Then use this Class as a base for your Controllers.