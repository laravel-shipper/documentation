## Payments And Products

**Table of contents:**
1. [Set Up Payments](/doc/payments-and-products.md#1-set-up-payments)
2. [Create Products](/doc/payments-and-products.md#2-create-products)
3. [No free account / Force user payment or subscription](/doc/payments-and-products.md#3-no-free-account--force-user-payment-or-subscription)

### 1. Set Up Payments

Laravel Shipper use stripe for payments. So you will need an Stripe account before configure payments.

The first thing you will need is to **add your public, and secret key in your `.env` file**.

```env
STRIPE_KEY=pk_test_…
STRIPE_SECRET=sk_test_…
```

And you also need to **create the webhook in your stripe account** so we can sync subscription status and payments. To create this webhook just run:

```bash
php artisan cashier:webhook
```

#### Important: add `checkout.session.completed` event to the webhook

We need this event to confirm user payments so it's important to add it manually to the webhook you just create.

Go to stripe, and make sure that your webhook events maches this one.

![image](https://github.com/user-attachments/assets/164d64e2-e9e0-4e7d-b89a-b84092a21fee)

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

### 4. Check user subscriptions or payments

During the development of your app, you will need to check if the current user has an active subscrition or has purchase a product.

#### Check purchases

You have to method, in the `User` model, to check if it has purchase any product, or an specific product.

```php
$hasPurchasedAnyProduct = $user->productPurchased();

$hasPurchasedProductId = $user->productPurchased('premium');
```

Also, you can get all the product purchased by this user and do whatever you want with them:

```php
// A Collection of all user purchases
$purchases = $user->purchases;
```

#### Check subscriptions

With subscritions you can do something similar.

```php
$hasAnySubscriptionActive = $user->subscribed();

$isSubscribedToProductId = $user->subscribedToProduct('premium');
```

And you can also get all user subscriptions and do what you need with them:

```php
// A Collection of all user subscriptions
$subscriptions = $user->subscriptions;
``
