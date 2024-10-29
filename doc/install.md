## Install & Quick Start Guide

**Table Of Content:**

1. Create a new Laravel Project
2. Install Laravel Shipper

### 1. Create a new Laravel Project

It's not mandatory, but it's better to start with a fresh new Laravel Application. You can create it in multiple ways just following the [official documentation](https://laravel.com/docs/11.x#creating-a-laravel-project) or with composer by just running:

```
composer create-project laravel/laravel example-app
```

**Before continue**, it's important to set up your [database connection](https://laravel.com/docs/11.x#databases-and-migrations) and make sure everything is working.


### 2. Install Laravel Shipper

One your Laravel project is working and you can access `http://localhost`, then you can continue by installing the project.

Before requiring the package, we need to config the repository and credentials:

```shell
# Setup composer package
composer config repositories.laravel-shipper composer https://laravelshipper.com

# Setup your email and token
composer config http-basic.laravelshipper.com john@doe.com your-api-token
```

And lastly, just require the package using composer.

```
composer require laravel-shipper/saas:^1.0
```

### 3. Set Up your Project

The package is required and importent in your new project. You just need to run the install command, and that's all!

```shell
php artisan shipper:install
```
