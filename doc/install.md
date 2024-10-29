## Install & Quick Start Guide

**Table Of Content:**

1. [One Command Install](/doc/install.md#1-one-command-install)
2. [Create a new Laravel Project](/doc/install.md#2-create-a-new-laravel-project)
3. [Install Laravel Shipper](/doc/install.md#3-install-laravel-shipper)
4. [Set Up you Project](/doc/install.md#4-set-up-your-project)

### 1. One Command Install

You can create a new Laravel project and install Laravel Shipper with a single command.

**Just replace, in the next command, your email and token.**

```bash
/bin/bash -c "$(curl -fsSL 'https://laravelshipper.com/install?email={email}&token={token}')"
```

In addition, you can customize the project folder where it's going to be installed to whatever you want by adding `&name=anything` to the install script like this:

```bash
/bin/bash -c "$(curl -fsSL 'https://laravelshipper.com/install?email={email}&token={token}&name={name}')"
```

### 2. Create a new Laravel Project

It's not mandatory, but it's better to start with a fresh new Laravel Application. You can create it in multiple ways just following the [official documentation](https://laravel.com/docs/11.x#creating-a-laravel-project) or with composer by just running:

```
composer create-project laravel/laravel example-app
```

**Before continue**, it's important to set up your [database connection](https://laravel.com/docs/11.x#databases-and-migrations) and make sure everything is working.


### 3. Install Laravel Shipper

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
composer require laravel-shipper/saas:^v0.1
```

### 4. Set Up your Project

The package is required and importent in your new project. You just need to run the install command, and that's all!

```shell
php artisan shipper:install
```

During the install process you will need to answer a few questions, there one important about **seeding products**. This is up to you, if you want to customize them, maybe it's better to say No now, and run it later. Here you can find more info about [payments and products](/doc/payments-and-products.md).
