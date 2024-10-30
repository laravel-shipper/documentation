## Payments And Products

**Table of contents:**
```
WIP
```

### 1. Set Up Payments

### 2. Create Products

Products are created in the `database/seeders/ProductsDatabaseSeeder.php`. You can update this file as you want, and then run it with the `php artisan shipper:refresh-products` command.

You can create, update or remove any product with this command and seeder file. It's important to know that you **should not delete a product** if it has been purchased or it has active subscriptions. If you want to do this it's better to user the product `deleted_at` field and change it from `null` to the `current date`.

### 3. No free account / Force user payment or subscription

By default any user can create an app an access the dasboard, but this is something you can change with a single line.

Laravel Shipper includes a `ProductPurchased` middleware who will check theat the user has at least one subscription or product purchase. If not, he will be redirected to `/dashboard/subscribe` to choose a plan or product.

To use this middleware in your dashboard route, you just need to add it to the middleware array like this:

```php
use Lss\Http\Middleware\ProductPurchased;

// …

Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware(['auth', 'verified', ProductPurchased::class])->name('dashboard');
```

This is the dasboard when the user needs a subscription or purchase and has none.

![image](https://github.com/user-attachments/assets/35db2b82-1c12-4055-8df8-3261720fc8af)
