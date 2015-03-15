### Creating Fields

To use your custom field on DataForm/DataEdit you need to specify the class namespace
```php
$form->add('hexcolor', 'Color', 'MyNameSpace\Fields\Colorpicker');
```
This class:
- must extend **\Zofe\Rapyd\DataForm\Field\Field**
- this class must have at least a property "type" for example $type = 'colorpicker';
- in this class you should override _build()_ method  to fill _$this->output_   based on  field 'status'
(field status is a property that dataform share with fields,  it can be: "show,create,modify, hidden")
- you can override (if needed): _getValue()_ and _getNewValue()_ to fill _$this->value_  and _$this->new_value_  
by default _$this->value_  is filled by the value stored in the db  and _$this->new_value_ will be the new value on save (is a little more complicated but you get the idea).

Most of time if field is "simple" (doesn't require transformations) you need only to override build().
Like this one:
https://github.com/zofe/rapyd-laravel/blob/master/src/Zofe/Rapyd/DataForm/Field/Redactor.php


If you don't know how to setup a custom Namespace in Laravel this is a mini how-to:

### Custom Namespace in Laravel for managing fields

- make a \app\Yourname  folder 
- make a \app\Yourname\Field  folder


in this last folder put your fields:
```php
<?php namespace Yourname\Field;
use Illuminate\Support\Facades\Form;
use Zofe\Rapyd\Rapyd;
use Zofe\Rapyd\DataForm\Field\Field;

class Myfield extends Field {
....
```
- in your main (laravel) composer.json add this declaration:
```json
"autoload": {
    "psr-0": {
           "Yourname": "app/"
    }
},
```
- then you must regenerate autoload by 
```sh
composer dump-autoload
php artisan dump-autoload
```
- then you should be able to use your own fields:
```php
$form->add('customfield', 'Asdasd', 'Yourname\Field\Myfield');
```

obviously if you need to include js/css assets you can put it on a /public/yourname/ folder then reference it in your field using:
```php
Rapyd::js('yourname/myfield.min.js');
Rapyd::css('yourname/myfield.css');
``
