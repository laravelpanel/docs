# FAQ

Q: Is it compulsory to add 'created_at' and 'updated_at' fields in CRUD item table ?
A: No, it's not compulsory. If you don't need these fields, just add the following line to your model :

	public $timestamps = false;

Q: How can I create a drop down field with reference to another CRUD item. Also how can I show that field in list of records ?
A: You can use the 'options' method to pass the values from another table to drop down fields. This method gets an array of key/values, for example if the name    of the model you want to get the values from is 'Company', you can define your field like this :

	$this->edit->add('field-name', 'label', 'select')->options(\Company::lists("name", "id"));

Q: How can I display/edit data which is fetched from more than one table in CRUD ?
A: If for example we need to obtain `material` and `cateogry` of each `product`, we need to modify the grid for this :

	$grid = DataGrid::source(Product::with('category','material');

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
