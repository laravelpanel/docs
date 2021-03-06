## Elfinder 

We are using barryvdh/laravel-elfinder 0.3.4

In your config/elfinder.php, you can change the default folder, the access callback or define your own roots.

### Views

You can override the default views by copying the resources/views folder. You can also do that with the `vendor:publish` command:

    php artisan vendor:publish --provider='Barryvdh\Elfinder\ElfinderServiceProvider' --tag=views

### Using Filesystem disks

Laravel 5 has the ability to use Flysystem adapters as local/cloud disks. You can add those disks to elFinder, using the `disks` config.

This examples adds the `local` disk and `my-disk`:

    'disks' => [
        'local',
        'my-disk' => [
            'URL' => url('to/disk'),
            'alias' => 'Local storage',
        ]
    ],
    
You can add an array to provide extra options, like the URL, alias etc. [Look here](https://github.com/Studio-42/elFinder/wiki/Connector-configuration-options-2.1#root-options) for all options.

### Using Glide for images

See [elfinder-flysystem-driver](https://github.com/barryvdh/elfinder-flysystem-driver) for [Glide](http://glide.thephpleague.com/) usage. A basic example with a custom Laravel disk and Glide would be:

1. Add the disk to your apps config/filesystems disks:

```
'public' => [
    'driver' => 'local',
    'root'   => base_path().'/public',
],
```
        
> Tip: you can use the `extend` method to register your own driver, if you want to use non-default Flysystem disks

2. Create a Glide Server for your disk, eg. on the `glide/<path>` route, using a cache path:

```
    Route::get('glide/{path}', function($path){
        $server = \League\Glide\ServerFactory::create([
            'source' => app('filesystem')->disk('public')->getDriver(),
            'cache' => storage_path('glide'),
        ]);
        return $server->getImageResponse($path, Input::query());
    })->where('path', '.+');
```
        
4. Add the disk to your elfinder config:

```
    'disks' => [
        'public' => [
            'glideURL' => '/glide',
        ]
    ],
```

5. (Optional) Add the `glideKey` also to the config, and verify the key in your glide route using the Glide HttpSignature.

You should now see a root 'public' in elFinder with the files in your public folder, with thumbnails generated by Glide.
URLs will also point to the Glide server, for images.