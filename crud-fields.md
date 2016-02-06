# CRUD Fields

Here we explain how you can add different types of fields to your CRUD interface. The Controller files contain 2 methods; 

- all - Within this method you can modify the grid
- edit - Within this method you can modify the edit form 

### Grid View

- Filtering and Grid Fields
```php
// Filter 
$this->filter = \DataFilter::source(new \App\Product());
$this->filter->add('categoryId','Category','select')->options(\App\Category::lists("name", "id")->all()); // Filter with Select List
$this->filter->add('name', 'Name', 'text'); // Filter by String
$this->filter->submit('search');
$this->filter->reset('reset');
$this->filter->build();

// Grid Columns
$this->grid = \DataGrid::source($this->filter);
$this->grid->add('name', 'Name');
$this->grid->add('description', 'Description');
$this->addStylesToGrid();
```
- Paginate
```php
$this->grid->paginate(10);
``` 
- Ordering
```php
$this->grid->add('name', 'Name', true); // allow ordering by this column
$this->grid->orderBy('article_id','desc'); //default orderby
``` 


### Edit View
- Allow Sorting on column
```php
$this->grid->add('name', 'Name', true);
``` 


- text (input field) 
```php
$this->edit->add('title', 'Title', 'text'); // field name, label, type
$this->edit->text('title', 'Title'); // field name, label (short syntax)
```
- select box 
```php
$this->edit->add('company_id','Company','select')->options(\App\Company::lists("name", "id")->all());
$this->edit->add('adminId','Admin','select')->insertValue(1)->options(\Serverfireteam\Panel\Admin::lists("email", "id")->all());  // pre-select a value in a select box
```
- checkbox (usually used for boolean (0 or 1) values,  but it can be configured on different values (to be usable also for enum fields))
```php
$this->edit->add('public', 'Public', 'checkbox');
$this->edit->checkbox('title', 'Title'); // field name, label (short syntax)
```
- checkboxgroup (checkbox list), usually using relation.fieldname to store values in a pivot table, for a belongsTo relation
```php
$this->edit->add('categories', 'Categories', 'checkboxgroup')
     ->options(Category::lists("name", "category_id")->all());
```
- radiogroup, as in checkboxgroup you can use option or options() to fill values
```php
$this->edit->add('gender', 'Gender', 'radiogroup')
     ->option('F', 'Female')->option('M', 'Male');
```
- textarea (textarea field)
```php
$this->edit->add('body', 'Body', 'textarea')->rule('required'); // validation
```
- redactor (textarea rendered as **wysiwyg** via redactor mit lic. version 7.6.1 old but good)
```php
$this->edit->add('body', 'Body', 'redactor'); 
```
- file upload field, use move(path, [filename]) to change filename and destination  
```php
$this->edit->add('download', 'Attachment', 'file')->rule('mimes:pdf')->move('uploads/pdf/');
```
- image (file field with some image utils bundled using [intervention image](https://github.com/Intervention/image): preview, resize, crop, ...
```php
$this->edit->add('photo', 'Photo', 'image')->move('uploads/images/')->preview(80,80);
```
- autocomplete (input field with autocomplete feature using [twitter typeahead](https://twitter.github.io/typeahead.js/) and ajax features  using relation.fieldname and search() method
```php
$this->edit->add('author.fullname', 'Author', 'autocomplete')
     ->search(array('firstname', 'lastname'));
```
- tags (input field with multiple autocomplete feature, using twitter typeahead and [TagsInput](https://github.com/TimSchlechter/bootstrap-tagsinput)
```php
$this->edit->add('categories.name', 'Categories', 'tags'); 
```
- colorpicker (input field with colorpicker for hex values [bootstrap-colorpicker](http://mjolnic.github.io/bootstrap-colorpicker/))
```php
$this->edit->add('color', 'Color', 'colorpicker'); 
```

- date:  simple date field (https://github.com/eternicode/bootstrap-datepicker/) 
It support many locales (ie: 'it','en','de' [complete list](https://github.com/eternicode/bootstrap-datepicker/tree/master/js/locales)), and some formats (ie: 'm/d/Y', 'd.m.Y', etc. you can use only "m","d","Y" for now).
```php
$this->edit->add('publication_date', 'pub.date', 'date')->format('d/m/Y', 'it');
```
- daterange:  two date field that works together to make a WHERE BETWEEN, to be used on filters.
It supports some locales & formats like date field
```php
$this->filter->add('publication_date', 'pub.date', 'daterange')->format('d/m/Y', 'it');
```

- auto:  a simple way to pass values to your model without output, you must use insertValue() and/or updateValue(). 
```php
$this->filter->add('myfield', '', 'auto')->insertValue('myvalue');
$this->filter->add('anotherfield', '', 'auto')->updateValue('anothervalue');
```
