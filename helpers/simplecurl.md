# SimpleCurl

The SimpleCurl class is there to curl data from RESTful services. A lot of companies use it nowadays for example twitter, google and facebook.

There are four methods available these are get, post and put.

 **You will need to declare the SimpleCurl helper first to use these examples below. You can do it by adding a use statement at the top of the controller.**

````
use Helpers\\SimpleCurl as Curl
````

**How to do a get request**

This example will show you how to a get request to get the current bitcoin prices from coinbase

````
// Get the spot price of a bitcoin it returns a json object.
$spotrate = Curl::get('https://coinbase.com/api/v1/prices/spot_rate');
$data['spotrate'] = json_decode($spotrate);
````

The get request returned the data as json data we encoded it and passed it to our view.

Inside your view you could simply do

````
echo $data['spotrate']->amount;
echo $data['spotrate']->currency;
````

This should print out the currency and rate.

**How to do a post request**

This example will show you how to post a gist to github gists.

````   
// Post a gist to github
$content = "Hello World!";
$response = Curl::post('https://api.github.com/gists', json_encode(array(
  'description' => 'PHP cURL Post Test',
  'public' => 'true',
  'files' => array(
    'Test.php' => array(
      'content' => $content,
    ),
  ),
)));    
````

The response will be details of the file and the url where it's located.

**How to do a put request**

This example will show you how to do a put request to httpbin a test service for curl.

````   
$response = Curl::put('http://httpbin.org/put', array(
  'id' => 1,
  'first_name' => 'Simple',
  'last_name' => 'MVC'
)); 
````