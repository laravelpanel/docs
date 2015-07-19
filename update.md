## Update

To update the panel to the latest version, you need to :

1. Change the version of the package to the latest version in require section of composer :

    ```json
    {
        "require": {
            "serverfireteam/panel": "1.3.*"
        },
    }
    ```

And run the composer update command, the package and its dependencies will be installed.

2. Then run the following command in order to publish latest configs, views and assets.

    ```bash
    php artisan panel:install

    ```
