# CRUD Field Validation

In order to add a validation rule to each field in CRUD, you just need to call the 'rule' function by passing the validation rule, for example :

```php
$this->edit->add('title', 'Title', 'text')->rule('required'); // A required field

$this->edit->add('user_id', 'User ID', 'text')->rule('exists:users,id'); // The value of this field should exist in users table (id column)
```
