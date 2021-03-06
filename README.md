# Simple Html Form Builder

This object oriented form builder will allow you to share form objects between different pages in your project.
Ideally via some form of dependency injection.

## Why?

Lazy. That's why.
This package hopefully will bring us one step closer to fully interoperable systems.

## Usage

For instance in Slim you can do the following.

```php
use Geggleto\Forms\Form;
use Geggleto\Forms\Input;

$container = $app->getContainer();
$container['loginForm'] = function ($c) {
    $generator = $c['BootstrapGenerator'];
    
    return $generator->generate([
        [
            'label' => 'Email',
            'type' => 'email',
            'id' => 'inputEmail3',
            'placeholder' => 'Email'
        ],
        [
            'label' => 'Password',
            'type' => 'password',
            'id' => 'inputPassword3',
            'placeholder' => 'Password'
        ],
    ], 'Sign In');
};

```


## Real world Usage

### Example Bootstrap Login Form

```php
use Geggleto\Forms\Button;
use Geggleto\Forms\Div;
use Geggleto\Forms\Form;
use Geggleto\Forms\Input;
use Geggleto\Forms\Label;

$form = (new Form())
    ->setAttribute('class', 'form-horizontal')
    ->addChild(
        (new Div())
            ->setAttribute('class', 'form-group')
            ->addChild(
                (new Label())
                    ->setAttribute('for', 'inputEmail3')
                    ->setAttribute('class', 'col-sm-2 control-label')
                    ->setInnerHtml('Email')
            )
            ->addChild(
                (new Div())
                    ->setAttribute('class', 'col-sm-10')
                    ->addChild( 
                        (new Input())
                            ->setAttribute('type', 'email')
                            ->setAttribute('class', 'form-control')
                            ->setAttribute('id', 'inputEmail3')
                            ->setAttribute('placeholder', 'Email')
                    )
            )
    )->addChild(
        (new Div())
            ->setAttribute('class', 'form-group')
            ->addChild(
                (new Label())
                    ->setAttribute('for', 'inputPassword3')
                    ->setAttribute('class', 'col-sm-2 control-label')
                    ->setInnerHtml('Password')
            )
            ->addChild(
                (new Div())
                    ->setAttribute('class', 'col-sm-10')
                    ->addChild(
                        (new Input())
                            ->setAttribute('type', 'password')
                            ->setAttribute('class', 'form-control')
                            ->setAttribute('id', 'inputPassword3')
                            ->setAttribute('placeholder', 'Password')
                    )
            )
    )->addChild(
        (new Div())
            ->setAttribute('class', 'form-group')
            ->addChild(
                (new Div())
                    ->setAttribute('class', 'col-sm-offset-2 col-sm-10')
                    ->addChild(
                        (new Button())
                            ->setAttribute('type', 'submit')
                            ->setAttribute('class', 'btn btn-default')
                            ->setInnerHtml('Sign in')
                    )
            )
    );
```

### Or you can use the Bootstrap Helper!
```php
$form = (new Form())
    ->setAttribute('class', 'form-horizontal')
    ->addChild(
        $factory->makeFormInput('Email', 'email', 'inputEmail3', 'Email')
    )->addChild(
        $factory->makeFormInput('Password', 'password', 'inputPassword3', 'Password')
    )->addChild(
        $factory->makeButton('Sign In')
    );
```

### Now if you wanted to use something like Vue.Js
```php
$form = (new Form())
    ->setAttribute('class', 'form-horizontal')
    ->addChild(
        $factory->makeFormInput('Email', 'email', 'inputEmail3', 'Email')
            ->setAttribute('value', '{{ model.email }}');
    )->addChild(
        $factory->makeFormInput('Confirm Email', 'email', 'inputEmail2', 'Email Again')
            ->setAttribute('value', '{{ model.email_confirm }}');
    )->addChild(
        $factory->makeButton('Sign In')
    );
```

### Or maybe you want to output the forms to html files.
```php

$factory = new Factory(); //bootstrap factory

$email = $factory->makeFormInput('Email', 'email', 'inputEmail3', 'Email');

//Traverse the Structure to get the Input Element
$email->getChild(1)->getChild(0)->setAttribute('value', '{{ model.email }}');

$form = (new Form())
    ->setAttribute('class', 'form-horizontal')
    ->addChild(
        $email
    )->addChild(
        $factory->makeFormInput('Password', 'password', 'inputPassword3', 'Password')
    )->addChild(
        $factory->makeButton('Sign In')
    );

    
$formHtml = $form->render();    
//Write it to disk... could be written to phtml or even twig
```

## Data Binding
It is now possible to bind data to your form.

```php
$form = (new Form())
    ->setAttribute('class', 'form-horizontal')
    ->addChild(
        $email
    )->addChild(
        $password
    )->addChild(
        $factory->makeButton('Sign In')
    )->setData([
        "email" => 'test@test.com',
        "password" => "123456"
    ]);
```

## Validation

Validation should ultimately be handled by your UI and your API.

## Generators

It is now possible to create forms via an array 
```php
       $factory = new Factory(); //bootstrap factory
        
        $generator = new Generator($factory);
        $form2 = $generator->generate([
            [
                'label' => 'Email',
                'type' => 'email',
                'id' => 'inputEmail3',
                'placeholder' => 'Email'
            ],
            [
                'label' => 'Password',
                'type' => 'password',
                'id' => 'inputPassword3',
                'placeholder' => 'Password'
            ],
        ], 'Sign In');
        
        // is equivilant too
        

        $form = (new Form())
            ->setAttribute('class', 'form-horizontal')
            ->addChild(
                $factory->makeFormInput('Email', 'email', 'inputEmail3', 'Email')
            )->addChild(
                $factory->makeFormInput('Password', 'password', 'inputPassword3', 'Password')
            )->addChild(
                $factory->makeButton('Sign In')
            );        
        
```


## Todo

### v1 goals
 - [ ] Add more Html Elements
 - [ ] Add Plugin to AJAXify the form
 - [ ] Add Plugin to use Vue.JS for binding


### v2 Goals
- [ ] Figure out a way to populate select options for the generator

