# Form Builder

- [Introduction](#introduction)
- [Basic Usage](#basic-usage)
    - [Open](#open)
    - [Close](#close)
    - [Model](#model)
    - [Label](#label)
    - [Text](#text)
    - [Password](#password)
    - [Email](#email)
    - [Hidden](#hidden)
    - [Textarea](#textarea)
    - [Select](#select)
    - [Checkboxes and Radio Buttons](#checkboxes-and-radio-buttons)
    - [Number](#number)
    - [File](#file)
    - [Buttons](#buttons)
    - [Reset](#reset)
    - [Image](#image)
    - [Url](#url)

<a name="introduction"></a>
## Introduction

The form builder is a useful tool where you can create forms easily. To get started on using the form buidler, you can instantiate it by calling the namespace.

```php
use Form;
```

<a name="basic-usage"></a>
## Basic Usage

<a name="open"></a>
### Open
To open a form you can use:

```php
echo Form::open([
    'method' => 'POST',
    'action' => 'App\Controllers\ControllerName@method'
]);
```

For example, if you were to create a form which would create a new post, you would use the following:

```php
echo Form::open([
    'method' => 'POST',
    'action' => 'App\Controllers\Admin\Post@store'
]);
```
> The `action` key would be your route. So in this instance our route in our routes file is: `Route::post('admin/posts', array('before' => 'auth|csrf', 'uses' => 'App\Controllers\Admin\Posts@store'));`

If you wanted to shorten the action, you could also use a route alias. So in the above example, our route could look like this:

```php
Route::post('admin/posts', array('before' => 'auth|csrf', 'uses' => 'App\Controllers\Admin\Posts@store', 'as' => 'admin.posts.store'));
```

and our `open` form can be changed to this:

```php
echo Form::open([
    'method' => 'POST',
    'action' => 'admin.posts.store'
]);

// Passing an id for an action, while using an alias:

echo Form::open([
    'method' => 'POST',
    'action' => [
        'admin.posts.update',
        $post->id
    ]
]);
```

Alternatively, if you wanted to use a url instead, you can use the following:

```php
echo Form::open([
    'method' => 'POST',
    'url' => site_url('admin/posts')
]);
```

These will all ouput the same:

```html
<form method="POST" action="http://www.novaframework.dev/admin/posts" accept-charset="UTF-8"><input name="csrfToken" type="hidden" value="Cn9Vt8Z9F5fgrxwSANd3g73oWQiFl0suJlB6IfP7">
```

Adding `'files' => true` to the `open` method will allow the form to accept file uploads.

```php
echo Form::open([
    'method' => 'POST',
    'url' => site_url('admin/posts').
    'files' => true
]);
```

Output:

```html
<form method="POST" action="http://www.novaframework.dev/admin/posts" accept-charset="UTF-8" enctype="multipart/form-data"><input name="csrfToken" type="hidden" value="Cn9Vt8Z9F5fgrxwSANd3g73oWQiFl0suJlB6IfP7">
```

<a name="close"></a>
### Close
To close a form you may use:

```php
echo Form::close();
```

Output:

```html
</form>
```

<a name="model"></a>
### Model

`Form::model()` is useful for forms that allow your users or yourself to edit already existing data in your database, as it will auto populate your fields for you.

> Keep in mind that you would need to name your fields the same as the columns in your database for this to work and you would need to pass the data via a variable from your controller.

```php
$post = Post::find($id);

return $this->getView()
    ->with('post', $post);
```

Then in your view:

```php
echo Form::model($post, [
    'method' => 'POST',
    'action' => [
        'admin.posts.update', 
        $post->id
    ]
]);
```
<a name="label"></a>
### Label

To create a label:

```php
echo Form::label('title', 'Post Title');
```

Output:

```html
<label for="title">Post Title</label>
```

<a name="text"></a>
### Text

To create a Text Field:

```php
echo Form::text('title', Input::old('title'), array('class' => 'form-control', 'id' => 'title'));
```

Output:

```html
<input class="form-control" id="title" name="title" type="text" value="[...]">
```

> Where [...] will be from the database, if anything.

<a name="password"></a>
### Password
To create a Password Field:

```php
echo Form::password('title', array('class' => 'form-control', 'id' => 'password'));
```

Output:

```html
<input class="form-control" id="password" name="password" type="password" value="">
```

<a name="email"></a>
### Email
To create an E-mail Field:

```php
echo Form::email('email', Input::old('email'), array('class' => 'form-control', 'id' => 'email'));
```

Output:

```html
<input class="form-control" id="email" name="email" type="email">
```

<a name="hidden"></a>
### Hidden
To create a Hidden Field:

```php
echo Form::hidden('hiddenField', Input::old('hiddenField'), array('class' => 'form-control', 'id' => 'hiddenField'));
```

Output:

```html
<input class="form-control" id="hiddenField" name="hiddenField" type="hidden">
```

<a name="textarea"></a>
### Textarea

To create a Textarea:

```php
echo Form::textarea('content', Input::old('content'), array('class' => 'form-control', 'id' => 'content'));
```

Output:

```html
<textarea class="form-control" id="content" name="content" cols="50" rows="10">[...]</textarea>
```

> Where [...] will be from the database, if anything.

<a name="select"></a>
### Select

To create a Select Field:

```php
echo Form::select('active', array('Y' => 'Yes', 'N' => 'No'), Input::old('active'), array('class' => 'form-control', 'id' => 'active'));
```

Output:

```html
<select class="form-control" id="active" name="active"><option value="Y">Yes</option><option value="N" selected="selected">No</option></select>
```

> `selected="selected"` will be assigned to whichever option is used from the database, if anything.

You can also use the following select elements:

```php
echo Form::selectMonth('month');
```

Output:

```html
<select name="month">
    <option value="1">January</option>
    <option value="2">February</option>
    <option value="3">March</option>
    <option value="4">April</option>
    <option value="5">May</option>
    <option value="6">June</option>
    <option value="7">July</option>
    <option value="8">August</option>
    <option value="9">September</option>
    <option value="10">October</option>
    <option value="11">November</option>
    <option value="12">December</option>
</select>

```

```php
echo Form::selectYear('year', 2010, 2016);
```

Output:

```html
<select name="year">
    <option value="2010">2010</option>
    <option value="2011">2011</option>
    <option value="2012">2012</option>
    <option value="2013">2013</option>
    <option value="2014">2014</option>
    <option value="2015">2015</option>
    <option value="2016">2016</option>
</select>

```

```php
echo Form::selectRange('numbers', 10, 20);
```

Output:

```html
<select name="range">
    <option value="10">10</option>
    <option value="11">11</option>
    <option value="12">12</option>
    <option value="13">13</option>
    <option value="14">14</option>
    <option value="15">15</option>
    <option value="16">16</option>
    <option value="17">17</option>
    <option value="18">18</option>
    <option value="19">19</option>
    <option value="20">20</option>
</select>
```

<a name="checkboxes-and-radio-buttons"></a>
### Checkboxes and Radio Buttons

Generating a checkbox or radio input

```php
echo Form::checkbox('name', 'value');

echo Form::radio('name', 'value');
```

Output:

```html
<input name="name" type="checkbox" value="value">

<input name="name" type="radio" value="value">
```

Generating a checkbox or radio input that is checked
```php
echo Form::checkbox('name', 'value', true);

echo Form::radio('name', 'value', true);
```

Output:

```html
<input checked="checked" name="name" type="checkbox" value="value">

<input checked="checked" name="name" type="radio" value="value">
```

<a name="number"></a>
### Number

To create a Number Field:

```php
echo Form::number('name', Input::old('name'), array('class' => 'form-control', 'id' => 'name'));
```

Output:

```html
<input class="form-control" id="name" name="name" type="number">
```

<a name="file"></a>
### File

To create a File upload field:

```php
echo Form::file('image');
```

Output:

```html
<input name="image" type="file">
```

> You must have the `Form::open()` `files` option set to `true`

<a name="buttons"></a>
### Buttons

```php
echo Form::submit('Click Me!');
```

or

```php
echo Form::button('Click Me!');
```

<a name="reset"></a>
### Reset

Adds a reset input field, which will reset the whole form, before any changes where made.

```php
echo Form::reset('reset', array('class' => 'form-control', 'id' => 'active');
```

Output:

```html
<input class="form-control" id="reset" type="reset" value="reset">
```

<a name="image"></a>
### Image

Adds a image input element to the form.

```php
echo Form::image('http://placehold.it/350x150', 'name', array());
```

Output:

```html
<input src="http://placehold.it/350x150" name="name" type="image" id="name">
```

<a name="url"></a>
### Url

Adds a url input field to the form.

```php
echo Form::url('name');
```

Output:

```html
<input name="name" type="url" id="name">
```