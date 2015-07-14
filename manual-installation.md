## Manual Installation

1. First you need a laravel 5 project ready to use.

2. Add the package to require section of composer :

    Laravel 5.0 :
    ```json
    {
        "require": {
            "serverfireteam/panel": "1.2.*"
        },
    }
    ```

    Laravel 5.1 :
    ```json
    {
        "require": {
            "serverfireteam/panel": "1.3.*"
        },
    }
    ```

And run the composer update command, the package and its dependencies will be installed.

3. Add the ServiceProvider of the package to the list of providers in the file config/app.php :

    Laravel 5.0 :
    ```php
    'providers' => array(
        'Serverfireteam\Panel\PanelServiceProvider'
    )
    ```

    Laravel 5.1 :
    ```php
    'providers' => array(
    	Serverfireteam\Panel\PanelServiceProvider::class
    )
    ```

4. Run the following command in order to publish configs, views and assets :

    ```bash
    php artisan vendor:publish
    ```

5. Go to the root of your project and run this command in order to set up the database :

    ```bash
    php artisan migrate --path="vendor/serverfireteam/panel/src/database/migrations"
    ```

6. Go to your domain.com/public/panel and you can login with the following username and password :

 * user : admin@change.me
 * password : 12345
