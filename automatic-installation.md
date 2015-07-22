## Automatic Installation

Note: if you face any problem in any of the steps you should report it at [github](https://github.com/serverfireteam/panel/issues/new)

1. First you need to create a laravel 5 project.

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

3. Add the ServiceProvider of the package to the list of providers in the config/app.php file

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

4. Run the following command in order to publish configs, views and assets.

    ```bash
    php artisan panel:install

    ```

5. Go to your domain.com/panel and you can login with the following username and password :

    user : admin@change.me

    password : 12345
