# AutoMapper+
An automapper for PHP inspired by [.NET's automapper](https://github.com/AutoMapper/AutoMapper).
Transfers data from one object to another, allowing custom mapping operations.

## Table of Contents
* [Installation](#installation)
* [Why?](#why)
* [Usage](#usage)
    * [Basic example](#basic-example)
    * [Custom callbacks](#custom-callbacks)
* [Roadmap](#roadmap)

## Installation
This is an alpha release, missing a lot of functionality and bound to be 
refactored. The package will be made available on Packagist once the initial
roadmap is completed ([see below](#roadmap)).

## Why?
When you need to transfer data from one object to another, you'll have to write 
a lot of boilerplate code. For example when using view models, CommandBus 
commands, response classes, etc.

Automapper+ helps you by automatically transferring properties from one object 
to another, including private ones. By default, properties with the same name
will be converted. This can be overriden as you like.

## Usage

### Basic example
Suppose you have a class Todo for which you want to create a view model for 
updating it. 

```php
<?php

class ToDo
{
    private $id;
    private $title;
    private $created;
    
    public function getId()
    {
        return $this->id;
    }
    
    // ...
}

class UpdateToDoViewModel
{
    public $id;
    public $title;
}
```

Basic example:

```php
<?php

$config = new \AutoMapperPlus\Configuration\AutoMapperConfig();
// If we only need to convert properties with the same name, simply registering
// the mapping is enough.
$config->registerMapping(ToDo::class, UpdateToDoViewModel::class);
$mapper = new \AutoMapperPlus\AutoMapper($config);

// With this configuration we can start converting our objects.
$todo = new ToDo(10, "I'm a title!", new \DateTime());

// $updateTodo:
// - $id: 10
// - $title: "I'm a title!"
$updateTodo = $mapper->map($todo, UpdateToDoViewModel::class);
```

### Custom callbacks
If you need something more complex than transferring properties with the same 
name, you can provide a custom callback:

```php
<?php

$config = new \AutoMapperPlus\Configuration\AutoMapperConfig();
$config->registerMapping(ToDo::class, UpdateToDoViewModel::class)
    ->forMember('title', function (ToDo $source) {
        // You can put some custom conversions here. 
        return ucfirst($source->getTitle());
    });
```

## Roadmap
- [ ] Add tests
- [ ] Add PHP7.1 dependency to composer
- [ ] Add comments (more than just PHPDoc blocks)
- [ ] Add the ability to change the default conversion (e.g. first do camelcase converting instead of using the exact same name)
- [ ] Add the 'Operation' type, @see Mapping.php
- [ ] Copy as many usages from .net's automapper as possible
- [ ] Provide a more detailed tutorial
- [ ] Create a Symfony bundle
- [ ] Create a sample app demonstrating the automapper
- [ ] Check if Reflectionclass PrivateAccessore implementation is faster (https://gist.github.com/samsamm777/7230159)
