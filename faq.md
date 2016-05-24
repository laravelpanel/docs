# FAQ

Q: Is it compulsory to add 'created_at' and 'updated_at' fields in CRUD item table ? 

A: No, it's not compulsory. If you don't need these fields, just add the following line to your model :

	public $timestamps = false;


Q: How can I create a drop down field with reference to another CRUD item. Also how can I show that field in list of records ? 

A: You can use the 'options' method to pass the values from another table to drop down fields. This method gets an array of key/values, for example if the name    of the model you want to get the values from is 'Company', you can define your field like this :

	$this->edit->add('field-name', 'label', 'select')->options(\Company::lists("name", "id")->all());


Q: How can I display data which is fetched from more than one table in CRUD ? 

A: If for example we need to obtain `material` and `category` of each `product`, we need to modify the grid in `ProductController` and add this line :

	$grid = DataGrid::source(Product::with('category','material'));

In the `Product` model the following modifications should be made:

	class Product extends \Eloquent
	{
		protected $table = 'products';

		public function category() {
			return $this->belongsTo('Category', 'category_id');
		}

		public function material() {
			return $this->belongsTo('Material', 'material_id');
		}
	}

Q: How can I save data into two related tables in CRUD ?

A: You just need to add the fields of the related table by specifying their relation in field name section of 'add' method, for example :

     $this->edit = \DataEdit::source(new \App\ModelName());
     $this->edit->add('relation.fieldname', 'Field Label', 'text');

Q: How can I preprocess some data before storing it in CRUD ? What would be the suggested way of doing it ? 

A: You should use model events, for example for storing secure password values you can use the 'creating' model event :

	public function edit($entity) {

		parent::edit($entity);
		\App\Modelname::creating(function($data) {
			$data->password = Hash::make(\Request::input('password'));
		});

		$this->edit = \DataEdit::source(new Modelname());

		$this->edit->label('Edit User');

		$this->edit->add('password', 'Password', 'text');

		return $this->returnEditView();
	}

Q: How can I add a blank option in select box field in CRUD ? 

A: The options function gets an array so you can just add an index to the array before passing it.

To add a blank option to the Edit view:

	$options = Department::lists("name", "id")->all();
	array_unshift($options, '');

	$this->edit = \DataEdit::source(new Category());
	$this->edit->add('parent_id', 'Department', 'select')->options($options);

To add a blank option on the Search View:

	$options = Department::lists("name", "id")->all();
	$blank_option = array(""=>"Please select a department");
	$department_options = $blank_option + $options;
	
	$this->filter->add('parent_id', 'Department', 'select')->options($department_options);

This will code will produce a dropdown box that looks like this:

	<select>
		<option selected> Please select a deparment </option>
		<option value="1"> Accounting Department </option>
		<option value="2"> Finance Department </option>
		<option value="3"> Engineering Department </option>
		...
	</select>
	
Q: How can I remove item from admin menu? 

A: You can use link controller in admin and remove it from there or use phpmyadmin and remove the record from link table
