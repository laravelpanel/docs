## Get started

There is few small steps to have working admin panel , that would take less than 10min .

1. [Install the LaravelPanel](/docs/master/automatic-installation) .
2. login and test the LaravelPanel , you should see empty dashboard at this point.
3. [Make crud with crud commands ](/docs/master/crud-commands) .
4. [Add your fileds to crud controller you have created](/docs/master/crud-fields) .
5. Test your crud controller with going to yourdomain.com/panel  
6. [create custom contoller for other type of admin parts](/docs/master/customized-controller-view) (Optional)


## Panel Commands, files & namespaces
The LaravelPanel package adds three new artisan commands. 
These are `panel:crud`, `panel:createmodel`, and `panel:createcontroller`.

The first command `panel:crud` is a useful combination of the other two commands, 
and creates both a new model and a matching controller.
The separate `panel:createmodel` and `panel:createcontroller` commands provide the flexibility to create 
models and controllers in any location suited to the project structure.

For all LaravelPanel commands, if the entity name (only) is supplied then LaravelPanel will use *default* locations 
that match the base Laravel structure.
For models, this is the *app* folder, and for controllers, the *app/Http/Controllers* folder

###Examples###

`$ php artisan panel:crud Foo` will create a new model class `Foo`, and also a matching controller class `FooController`.
 The model file will be *app/Foo.php*. It  will be namespaced as `App\Foo`  and `class FOO extends Model;`
 The controller file will be *app/Http/Controllers/FooController.php*, with a namespace of  `App\Http\Controllers`
(Note: the model and the controller then need fleshing out with your code)

###Alternative locations for Models and/or Controllers###
The default locations for models and controllers may be just fine, especially for small projects!
But with a large number of models it may be preferred to keep models and controllers 
stored elsewhere to fit your project structure. 
For example, keeping all models in an *app/Models* folder.
This can easily be achieved using the two separate LaravelPanel commands provided.

For example, creating a Post model and PostController.
`panel:createmodel Models/Post` will create the model file *app/Models/Post* folder, namespaced to `App\Models\Post;` and 
`panel:createcontroller Blog/PostController` will create file *app/Http/Controllers/Blog/PostController.php*, again with a correct namespace.

If a **complete path** is supplied to the panel command (starting with /App), that will also be respected.
So `panel:createmodel /App/Blog/Post` and `panel:createcontroller /App/Blog/PostController` will create both the model 
and controller files in the *app/Blog/* folder.
