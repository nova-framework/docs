# Hooks 

**Add modules with hooks**

The hooks helper allows modules to be created within the module folder. Hooks allow code to be injected into various parts of the framework as well as creating new routes for modules to use.

The following hooks have been created and placed in the following files to allow you to make use of them.

- meta - placed in app/templates/default/header.php
- css - placed in app/templates/default/header.php
- afterBody - placed in app/templates/default/header.php
- footer - placed in app/templates/default/footer.php
- js - placed in app/templates/default/footer.php
- routes - placed in index.php

The hooks: meta, css, afterBody, footer and js are placeholders to execute code from modules. The routes hook enables custom routes to be defined from a module.

**Place a hook where it can be executed**

To use a hook first make reference to it:

````
use Helpers\Hooks;

$hooks = Hooks::get();
````

Once initiated calls can be called by running $hooks->run('hookName'), this will then run all code attached to the hook for instance to run all code attached to the css hook (used for injecting css files):

````
<?php

use Helpers\Hooks;

//initialise hooks
$hooks = Hooks::get();
?>
<!DOCTYPE html>
<html lang="<?php echo LANGUAGE_CODE; ?>">
<head>
    <?php
    //hook for plugging in css
    $hooks->run('css');
    ?>
````

**Add code to a hook**

To add code to a hook call Hooks::addHook() this method expects 2 parameters:
- The hook to inject into.</li>
- The path to the class, start with the namespace followed by the class then @method to be called.  

````
use Helpers\Hooks;

Hooks::addHook('css', 'Models\Demo\Controllers\Demo@css');
````

Once run any code inside the class method will be executed on the page where the hook is being called.

**Calling custom routes**

Define the routes hook and pass in a path to the class and method to be used.

````
use Helpers\Hooks;

Hooks::addHook("routes", 'Modules\\Demo\\Controllers\\Demo@routes');
````

Next in the class create an alias for Router. Next in the method the routes will be used in call Router followed by the route desired, instead of calling a controller use a closure to run the code against the route.

````
<?php
namespace Modules\Demo\Controllers;

use Core\Router;

class Demo
{
    public function routes()
    {
        Router::any('democall', function () {
            echo 'called from demo module';
        });
    }
}
````

In the above example a route is defined, it's called when the example.com/democall is ran in the address bar. This closure echo's a message. The code inside the closure can call views and models the same as controller methods do.

**Creating a Module**

To create a module create a folder inside the Modules folder, the convention is to use StudyCase for example a module called demo should name named Demo. Inside the module folder create a Controllers, Models and views folder, those folders should hold the classes for controllers and models. The views is for the output of the module pages.

index.php should also be created in the route of the module folder, this file defined all hooks the module will be attached to.

**Video Demo**

<iframe width="853" height="480" src="https://www.youtube.com/embed/6MvQCtm2pL8" frameborder="0" allowfullscreen></iframe>