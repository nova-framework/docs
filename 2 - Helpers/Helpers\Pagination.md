Break recordset into a series of pages

First create a new instance of the class pass in the number of items per page and the instance identifier, this is used for the GET parameter such as ?p=2
The set_total method expects the total number of records, either set this or pass in a call to a model that will return records then count them on return.
The method used to get the records will need a get_limit passed to it, this will then return the set number of records for that page.
Lastly a method called page_links will return the page links.

The model that uses the limit will need to expect the limit:
```php
public function get_contacts($limit){
    return $this->db->select('SELECT * FROM '.PREFIX.'contacts '.$limit);
}
```

Pagination concept
```php
//create a new object
$pages = new Paginator('10','p');

//set the total records, calling a method to get the number of records from a model
$pages->set_total( $this->_model->get_all_count() );

//calling a method to get the records with the limit set
$data['records'] = $this->_model->get_all( $pages->get_limit() );

//create the nav menu
$data['page_links'] = $pages->page_links();

//then pass this to the view, may be different depending on the system
$this->_view->render('index', $data);
```

Usage example:

```php
$pages = new Paginator('50','p');
$pages->set_total( count($this->_model->get_contacts()));
$data['records'] = $this->_model->get_contacts( $pages->get_limit() );
$data['page_links'] = $pages->page_links();
```
