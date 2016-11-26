# Cron

- [Setup](#setup)
- [The Adapter](#the-adapter)
- [Registering the Cron](#registering-the-cron)
- [Run Cron Run](#run-cron-run)

<a name="setup"></a>
## Setup

Creating a new Cron is fairly simple to do. To keep things organised, it would be best to create a new folder in your `app` directory called `Cron` and then in that folder, make another folder called `Adapters`. You should have something like this:

- app
    - Cron
        - Adapters

Then under your adapters folder, you can create your cron adapter, For the purposes of this guide, you will create an adapter called `HelloWorld.php` which will return 'Hello World'.

In order to activate the cron, you would need to setup a token in `app/Modules/System/Config.php` by modifying the below configuration:

```php
use Nova\Config\Config;

/**
 * Configuration constants and options.
 */
Config::set('cron', array(
    /**
     * The CRON token.
     * This tool can be used to generate key - http://jeffreybarke.net/tools/codeigniter-encryption-key-generator
     */
    'token' => 'SomeRandomStringThere_1234567890',
));
```

<a name="the-adapter"></a>
## The Adapter
```php
namespace App\Cron\Adapters;

use Nova\Cron\Adapter;

class HelloWorld extends Adapter
{
    // The name of your Cron
    protected $name = 'Hello World';

    /**
     * Execute the CRON operations.
     */
    public function handle()
    {
        return 'Hello World!';
        // Or your complicated code here.
    }

}
```

<a name="registering-the-cron"></a>
## Registering the Cron
Then to register your cron, add the following line to `app\Bootstrap.php`

```php
Cron::register('App\Cron\Adapters\HelloWorld');
```

<a name="run-cron-run"></a>
## Run Cron Run
And finally, to run the cron, navigate to `www.novaframework.dev/cron/{token}`

You should receive something like this in your browser:

```
Nova 3.0 - Cron executed on 26 Nov 2016, 23:01

CRON Test : Hello from the CRON!

Hello World : Hello World!
```

That's it!