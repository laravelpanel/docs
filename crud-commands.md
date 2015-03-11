# CRUD Commands

- [panel::createmodel](#createmodel)
- [panel::createcontroller](#createcontroller)
- [panel::install](#install)
- [panel::crud](#crud)

<a name="createmodel"></a>
## panel::createmodel

To create the model of an entity, you should run the following command :

	php artisan panel:createmodel <entity-name>

<a name="createcontroller"></a>
## panel::createcontroller

To create the controller of an entity, you should run the following command :

	php artisan panel:createcontroller <entity-name>

<a name="install"></a>
## panel::install

To install the panel for your website you should run the following command :

	php artisan panel:install

This command publishes configs, views and assets and also sets up the database for the panel.

<a name="crud"></a>
## panel::crud

To create a CRUD, you should run the following command :

	php artisan panel:crud <entity-name>

This command creates the model and controller of an entity. Before running this command, you should create the table.
