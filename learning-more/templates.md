- [Introduction](#introduction)
- [Functions](#functions)
- [Routing](#routing)
    - [CSS](#css)
    - [JS](#js)
    - [Images](#images)
- [Template Syntax](#template-syntax)
- [Other Control Structures](#other-control-structures)

<a name="introduction"></a>
## Introduction

Templates are the site's markup, where `images`, `js`, and `css` files are located as well as the site's html structure. The default template is called `Default`.

Templates are located in the `theme` folder.

A regular site would have multiple css files, a javascript folder containing js files and an image folder. The rest of the site's pages come from the views.

Typical elements of a page include the following. The title comes `View::shares` set in controllers, this lets the controllers set the page titles in an array that is passed to the template. The site title `Config::get('app.name', SITETITLE)` comes from `app/Config/App.php`

From within a template your css, js and images must be in an `Assets` folder to be routed correctly. This applies to Modules as well. To grab a css file from a Module, the css file must be placed inside `modules/ModuleName/Assets/css/file.css`. 

Additionally, there is an Assets folder in the root of nova. This is for storing resources outside of templates which can still be routed from above the document root.

> **Note** Template folder names less then 4 characters should be in capital letters ie **Scf** should be **SCF** or resources will not be found. Folders such as Default are OK.

<a name="functions"></a>
## Functions

`vendor_url()` accepts two params:
- path to the resource
- the name of the dependency in the composer's `vendor` directory.
```php
vendor_url('dist/css/bootstrap.min.css', 'twbs/bootstrap')
```

`template_url()` accepts two params:
- path to the image file relative to the theme's root.
- the theme name to be used

```php
<img src='<?= template_url('images/nova.png', 'Default'); ?>' alt='logo'>
```

`resource_url()` accepts two params:
- path to the resource
- the name of the module (optional)

```php
<img src='<?= resource_url('images/nova.png', 'Default'); ?>' alt='logo'>
```

<a name="routing"></a>
## Routing

<a name="css"></a>
#### CSS

CSS files can be loaded by using `Assets::css()` and passing in an array of css paths.

```php
Assets::css([
    vendor_url('dist/css/bootstrap.min.css', 'twbs/bootstrap'),
    vendor_url('dist/css/bootstrap-theme.min.css', 'twbs/bootstrap'),
    'https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css',
    template_url('css/style.css', 'Default'),
]);
```

<a name="js"></a>
#### JS

Loading JS is the same process as CSS, only this time its `Assets::js()`

```php
Assets::js([
    'https://code.jquery.com/jquery-1.12.4.min.js',
    vendor_url('dist/js/bootstrap.min.js', 'twbs/bootstrap'),
]);
```

<a name="images"></a>
#### Images

Since the images are above the document root, they have to be served from Nova. This is done by either using `template_url()` for images in the theme or `resource_url()` for resources in the global assets folder or from a module.

<a name="template-syntax"></a>
## Template Syntax


#### Defining A Layout

```php
<!-- Stored in themes/TemplateName/Layouts/Master.tpl.php -->

<html>
    <body>
        @section('sidebar')
            This is the master sidebar.
        @show

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```

#### Using A Layout

```php
@extends('layouts.master')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@stop

@section('content')
    <p>This is my body content.</p>
@stop
```

Note that views which `extend` a layout simply override sections from the layout. Content of the layout can be included in a child view using the `@parent` directive in a section, allowing you to append to the contents of a layout section such as a sidebar or footer.

Sometimes, such as when you are not sure if a section has been defined, you may wish to pass a default value to the `@yield` directive. You may pass the default value as the second argument:

`@yield('section', 'Default Content')`

<a name="other-control-structures"></a>
## Other Control Structures

#### Echoing Data

```
Hello, {{{ $name }}}.
The current UNIX timestamp is {{{ time() }}}.
```

#### Echoing Data After Checking For Existence

Sometimes you may wish to echo a variable, but you aren't sure if the variable has been set. Basically, you want to do this:

```
{{{ isset($name) ? $name : 'Default' }}}
```

However, instead of writing a ternary statement, Template allows you to use the following convenient short-cut:

```
{{{ $name or 'Default' }}}
```

#### Displaying Raw Text With Curly Braces

If you need to display a string that is wrapped in curly braces, you may escape the Template behavior by prefixing your text with an @ symbol:

```
@{{ This will not be processed by Template }}
```

Of course, all user supplied data should be escaped or purified. To escape the output, you may use the triple curly brace syntax:

```
Hello, {{{ $name }}}.
```

If you don't want the data to be escaped, you may use double curly-braces:

```
Hello, {{ $name }}.
```

>> Note: Be very careful when echoing content that is supplied by users of your application. Always use the triple curly brace syntax to escape any HTML entities in the content.

#### If Statements

```
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif

@unless (Auth::check())
    You are not signed in.
@endunless
```

#### Loops

```
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor

@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach

@forelse($users as $user)
    <li>{{ $user->name }}</li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

#### Including Sub-Views

```
@include('view.name')
```

You may also pass an array of data to the included view:

```
@include('view.name', array('some'=>'data'))
```

#### Overwriting Sections

To overwrite a section entirely, you may use the overwrite statement:

```
@extends('list.item.container')

@section('list.item.content')
    <p>This is an item of type {{ $item->type }}</p>
@overwrite
```

#### Comments

```
{{-- This comment will not be in the rendered HTML --}}
```