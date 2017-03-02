# In 3.76.0 a new template engine was added.

The native **Template Engine** responds to View files with the extension `.tpl`all views which will use the template syntax 
should be have the extension .tpl view files with .php won't be able to use the template engine.

> **Submime Text** users can install a highliter package to recognise the template syntax https://github.com/nova-framework/template-highlighter-sublime-text

## Control Structure

### Printing variables 

To display a variable it should be wrapped inside a set of `{{` and to close `}}`

```php
{{ $title }}
```

Variables wrapped inside {{ }} are **Not** escaped on output. All data user generated should always be escaped. 
When using user generated variables use:

```php
{{{ $title }}}
```

Using 3 curly braces **Will** escape and is safer to use.

Variables passed inside the braces will be executed no need to open php and close it after, this keeps the view files 
clean and flexible.

### Printing a variable or using a default

If a variable might not exist you can echo it and provide a default value:

```php
{{ $name OR 'no name provided' }}
```

This will result in $name being used if it exists otherwise the string 'no name provided' would be printed.

### if statements

```php
@if (count($items) == 0)
  <p>No items</p>
@elseif (count($items) < 5)
  <p>There are only {{{ count($items) }}} left.</p>
@else
  <p>Total items: {{{ count($items) }}}</p>
@endif
```

### For loops

```php
@for ($i = 0; $i < 10; $i++)
    The current value is {{ $i }}
@endfor
```

### Foreach Loops

```php
@foreach ($users as $user)
    <p>This is user {{ $user->id }}</p>
@endforeach
```

### While loops

```php
@while (true)
    <p>I'm looping forever.</p>
@endwhile
```

### Switch statements
```php 
@switch ($item) {
    case '1':
        # code...
        break;
    
    default:
        # code...
        break;
}
```

## Including views

To include other views into the existing view, specify the path starting from App\Views. Don't include the extension

```php
@include('Welcome/SubPage')
```

Optionally using an include from a module, when specifying the module name the view path will be from the module/views path.

```php
@include('Welcome/Home', [], 'ModuleName')
```
The 2nd param is an array to pass data to the view.

The reason why you not need to pass data on @include('Welcome/Home', 'ModuleName') is that ALL defined variables, local or pushed to View, are made available in the included TPL.

## Comments

To add a php comment:

```php
{{-- this is a php comment, it won't be printed. --}}
```

## Using raw php

There will be times where using php normally will be required you can so this by making use of @php:

```php
@php
$total = 0;
@endphp
```

<p>Total is {{ $total }}.</p>

## View Layouts

You can use any view inside another view so it makes sense that you can setup a master view for greater control over the layout of a set of views.

> **Note** this is exclusivly for Views no theme layouts can be used with `@include` or `@extend` for layout specific changes go to the theme layouts.

`@extends` should be used exclusively when you have a View skeleton, then populate with different variants.

The convention is to have a folder called layouts in **app\Views\Layouts** you are of course free to choose your own path.

For example setup a blog layout **app/Views/Layouts/BlogPosts.tpl**

Create a section of content that can be extended by using @section() give it a name and follow it with @show to print the results.

Anything inside will be displayed, this can be used on extended views to pass data into that section.

Another option is to use @yield() which is a placeholder to print any data passed to it.

> @yield() can have a default @yield('revisions', 'No revisions yet')

For example a layout view:

```php
<div class='row'>

    <div class='col-md-2'>
        @section('sidebar')
          Sample content
        @show
    </div>
    
    <div class='col-md-10'>
        @yield('content')
    </div>
    
</div>
```

An extended view:

```php
@extends('Layouts/BlogPosts')

@section('sidebar')
    @parent
    
    Some new sidebar content
@stop
```

The @parent will append the content of sidebar instead of replacing it.

```php
@section('content')
  @foreach ($posts as $post)
      <p>{{ $post->title }}</p>
  @endforeach
@stop
```
    
        
  
