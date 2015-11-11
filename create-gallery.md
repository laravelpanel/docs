# How to create an Image Gallery with managment system in 5 minutes
## Screen shot 
![clint side](http://laravelpanel.com/assets/img/create-gallery-1.png)
![managment part](http://laravelpanel.com/assets/img/create-gallery-2.png)


List of steps we need to follow :

1. [Install laravel](http://laravel.com/docs/5.1#installation)

2. [install laravelPanel](http://laravelpanel.com/docs/master/automatic-installation)

3. Create the files we need

4. Write the code in crud controller and test it

5. Fetch data on client side and show the gallery

if you want finished code you can download it from [here](https://github.com/serverfire/gallery)
# Create the files we need

After you have installed the Laravel and LaravelPanel from the links above, you need to create the crud for gallery items.

On your command line, go to the project directory and run the following command :

```
php artisan panel:crud gallery
```  
It will create your controller and model for gallery.

After that you need to create the migrations for your gallery table by running the following command :

```
php artisan make:migration create_gallery_table --table=gallery
```
Then go to galleryTest\database\migrations folder and open a file named like 2015_10_20_165012_create_gallery_table.php (Depending on the date, file name may be different)

We need to have a few fields in the gallery table so your migration file should look like this :

```php
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateGalleryTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('gallery', function (Blueprint $table) {
            $table->increments('id');
            $table->string('title');
            $table->string('subtitle');
            $table->string('image');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('gallery');
    }
}

```

Then run the migration to create the table :

```
php artisan migrate 
```

The output of this command should look like this :

```
Migrated: 2015_10_20_165012_create_gallery_table
```

And in phpmyadmin you should see the gallery table with four fields.

# Write the code in crud controller and test it

Open the galleryController.php from app\Http\Controllers and you'll see the following lines added inside the 'all' function :

```php
        /** Simple code of  filter and grid part , List of all fields here : http://laravelpanel.com/docs/master/crud-fields


			$this->filter = \DataFilter::source(new \App\Category);
			$this->filter->add('name', 'Name', 'text');
			$this->filter->submit('search');
			$this->filter->reset('reset');
			$this->filter->build();

			$this->grid = \DataGrid::source($this->filter);
			$this->grid->add('name', 'Name');
			$this->grid->add('code', 'Code');
			$this->addStylesToGrid();

        */
```

We need to uncomment the code above and change \App\Category to \App\Gallery.
We also need to change 'Name' field to 'Title' and 'code' field to 'image'.

The code of 'all' function should look like this :

```php
    public function all($entity) {

	parent::all($entity);

        $this->filter = \DataFilter::source(new \App\Gallery);
        $this->filter->add('title', 'Title', 'text');
        $this->filter->submit('search');
        $this->filter->reset('reset');
        $this->filter->build();

        $this->grid = \DataGrid::source($this->filter);
        $this->grid->add('title', 'Title');
        $this->grid->add('image', 'Image');
        $this->addStylesToGrid();

        return $this->returnView();
    }
```

We need to change part of the code in 'edit' function, we need to change the model name from \App\Category() to \App\Gallery(). We also need to add an other field for subtitle and change type of last field to 'file'. Your 'edit' function should look like this :

```php
    public function edit($entity) {

        parent::edit($entity);

        $this->edit = \DataEdit::source(new \App\Gallery());

        $this->edit->label('Edit Gallery');

        $this->edit->add('title', 'Title', 'text');

        $this->edit->add('subtitle', 'Subtitle', 'text');
		
        $this->edit->add('image', 'Image', 'file')->rule('required');
       
        return $this->returnEditView();
    }
```

After saving the file, you can go to admin panel (yourdomain.com/panel) and add some images to gallery and see how easy it is to have gallery manager with LaravelPanel.

# Fetch data on client side and show the gallery

First we need to fetch gallery data in route of our app and pass it to the 'welcome' view.

```php
Route::get('/', function () {
	$gallery = \App\Gallery::get();
	return view('welcome')->with('gallery', $gallery);
});

```

After that we need to add bootstrap js and css and jquery in header of welcome view located in resources\views\welcome.blade.php

```html
        <!-- Latest compiled and minified CSS -->
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">

        <!-- Optional theme -->
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap-theme.min.css">

        <!-- Latest compiled and minified jquery -->
        <script src="https://code.jquery.com/jquery-2.1.4.min.js"></script>

        <!-- Latest compiled and minified JavaScript -->
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js"></script>
```

After that we are going to use bootstrap carousel, we should just use default html from bootsrap site and add a loop like this :

```php
	<div id="carousel-example-generic" class="carousel slide" data-ride="carousel">

        	<!-- Wrapper for slides -->
		<div class="carousel-inner" role="listbox">

	                @for ($i = 0; $i < sizeOf($gallery); $i++)
        	        	<div class="item @if($i ==0) active @endif">
                			<img src="uploads/{{ $gallery[$i]->image }}" alt="{{ $gallery[$i]->title }}">
                  			<div class="carousel-caption">
		                	        <h3>{{ $gallery[$i]->title }}</h3>
                		        	<p>{{ $gallery[$i]->subtitle }}</p>
			                </div>
				</div>
                	@endfor
		</div>

		<!-- Controls -->
                <a class="left carousel-control" href="#carousel-example-generic" role="button" data-slide="prev">
	                <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
	                <span class="sr-only">Previous</span>
        	</a>
                <a class="right carousel-control" href="#carousel-example-generic" role="button" data-slide="next">
                	<span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
	                <span class="sr-only">Next</span>
		</a>
	</div>
```

Now when you open the first page of your site you should see the gallery images.

Hope you have enjoyed this tutorial.
