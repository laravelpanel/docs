# How to Create Image gallery in 5 minute 
To create a image gallery we need two part , Admin part ( for that we are going to use LaravelPanel ) and Client part .

List of steps we need to do
1. [Install laravel](http://laravel.com/docs/5.1#installation)
2. [install laravelPanel](http://laravelpanel.com/docs/master/automatic-installation)
3. Create files we need 
4. Write code in curd controller and test it  
5. Fetch data in client side show the gallery 


#Create files we need 
After you have install laravel and LaravelPanel from followed links , you need create the crud for galleries item .

Go to project directory and run 
```
php artisan panel:crud gallery
```  
in your Command Line .  it will create your controller and model for gallery .

After that you need create the migrations for your gallery table with :
```
php artisan make:migration create_gallery_table --table=gallery
```
Go to galleryTest\database\migrations  and open a file name like 2015_10_20_165012_create_gallery_table.php ( depend on date file name may be different ) 
 
 We need to have few fields in gallery table  , your migration file should be like this :
 
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

And run the migrate to create the table :
```
php artisan migrate 
```

Result should be like :
```
Migrated: 2015_10_20_165012_create_gallery_table
```

And in phpmyadmin you should see the gallery table with that 4 fields .

#Write code in curd controller and test it  
Open the galleryController.php from app\Http\Controllers you should see in all function : 
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

We need uncomment the code and change \App\Category to \App\Gallery
Name to title and code to image  

The code of all function should be like this :
```php
    public function all($entity){
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

We need to change some code in edit function  , we need change the model name from \App\Category() to  \App\Gallery() . Also we need add other filed for subtitle and change type of last filed to file . Your edit function should be like this
```php
    public function  edit($entity){
        
        parent::edit($entity);


			$this->edit = \DataEdit::source(new \App\Gallery());

			$this->edit->label('Edit Gallery');

			$this->edit->add('title', 'Title', 'text');

			$this->edit->add('subtitle', 'Subtitle', 'text');
		
			$this->edit->add('image', 'Image', 'file')->rule('required');



       
        return $this->returnEditView();
    }    
```
When you save that file you can go to admin panel ( yourdomain.com/panel ) and add gallery image and see how easy was to have gallery mangagment with LaravelPanel .

#Fetch data in client side show the gallery 

First we need fetch gallery data in route and pass it to welcome view 
```php
Route::get('/', function () {
	$gallery = \App\Gallery::get();
    return view('welcome')->with('gallery', $gallery);
});

```
After that we need add bootstrap js and css and jquery in header of welcome view in resources\views\welcome.blade.php
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
after that we are going use bootstrap carousel , just use default html from bootsrap site and put loop on it . like this : 
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

Now when you open first page of your site you should see the gallery images .

Hope you have enjoy this tutorial .
