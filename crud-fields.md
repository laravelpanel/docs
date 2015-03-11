# CRUD Fields

Here we explain how you can add different types of fields to your CRUD :

- text (input field) 
```php
$form->add('title', 'Title', 'text'); // field name, label, type
$form->text('title', 'Title'); // field name, label (short syntax)
```
- checkbox (usually used for boolean (0 or 1) values,  but it can be configured on different values (to be usable also for enum fields))
```php
$edit->add('public', 'Public', 'checkbox');
$edit->checkbox('title', 'Title'); // field name, label (short syntax)
```
- checkboxgroup (checkbox list), usually using relation.fieldname to store values in a pivot table, for a belongsTo relation
```php
$edit->add('categories', 'Categories', 'checkboxgroup')
     ->options(Category::lists("name", "category_id"));
```
- radiogroup, as in checkboxgroup you can use option or options() to fill values
```php
$edit->add('gender', 'Gender', 'radiogroup')
     ->option('F', 'Female')->option('M', 'Male');
```
- textarea (textarea field)
```php
$form->add('body', 'Body', 'textarea')->rule('required'); // validation
```
- redactor (textarea rendered as **wysiwyg** via redactor mit lic. version 7.6.1 old but good)
```php
$form->add('body', 'Body', 'redactor'); 
```
- file upload field, use move(path, [filename]) to change filename and destination  
```php
$edit->add('download', 'Attachment', 'file')->rule('mimes:pdf')->move('uploads/pdf/');
```
- image (file field with some image utils bundled using [intervention image](https://github.com/Intervention/image): preview, resize, crop, ...
```php
$form->add('photo', 'Photo', 'image')->move('uploads/images/')->preview(80,80);
```
- autocomplete (input field with autocomplete feature using [twitter typeahead](https://twitter.github.io/typeahead.js/) and ajax features  using relation.fieldname and search() method
```php
$form->add('author.fullname', 'Author', 'autocomplete')
     ->search(array('firstname', 'lastname'));
```
- tags (input field with multiple autocomplete feature, using twitter typeahead and [TagsInput](https://github.com/TimSchlechter/bootstrap-tagsinput)
```php
$form->add('categories.name', 'Categories', 'tags'); 
```
- colorpicker (input field with colorpicker for hex values [bootstrap-colorpicker](http://mjolnic.github.io/bootstrap-colorpicker/))
```php
$form->add('color', 'Color', 'colorpicker'); 
```

- date:  simple date field (https://github.com/eternicode/bootstrap-datepicker/) 
It support many locales (ie: 'it','en','de' [complete list](https://github.com/eternicode/bootstrap-datepicker/tree/master/js/locales)), and some formats (ie: 'm/d/Y', 'd.m.Y', etc. you can use only "m","d","Y" for now).
```php
$form->add('publication_date', 'pub.date', 'date')->format('d/m/Y', 'it');
```
- daterange:  two date field that works together to make a WHERE BETWEEN, to be used on filters.
It supports some locales & formats like date field
```php
$filter->add('publication_date', 'pub.date', 'daterange')->format('d/m/Y', 'it');
```

- auto:  a simple way to pass values to your model without output, you must use insertValue() and/or updateValue() 
```php
$filter->add('myfield', '', 'auto')->insertValue('myvalue');
$filter->add('anotherfield', '', 'auto')->updateValue('anothervalue');
```
