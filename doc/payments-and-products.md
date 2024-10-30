## Payments And Products

**Table of contents:**
1. [Set Up Payments](/doc/payments-and-products.md#1-set-up-payments)
2. [Create Products](/doc/payments-and-products.md#2-create-products)
3. [No free account / Force user payment or subscription](/doc/payments-and-products.md#3-no-free-account--force-user-payment-or-subscription)
4. [Check user subscriptions or payments](/doc/payments-and-products.md#4-check-user-subscriptions-or-payments)

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
```

### 5. Products in Pricing page

There are multiple ways to change the products that are shown on the pricing section.

The **simplest option** is to change the `products_filter` array in `config/shipper.php`. You can customize a type filter, the order, whether to show or not show unavailable products, and also the limit. For the most common use cases, this is more than enough.

```php
    'products_filter' => [
        // one-time, 'subscriptions', or null for all (default: null)
        'type' => null,

        // Show only one per product_id
        'group-by' => 'product_id',

        // Sort products by any product attribute (default: id)
        'order-by' => 'monthly_price_cents',

        // kow many unavailable products to show (default: 0)
        'unavailable' => 1,

        // How many products to show, it takes into account unavailable products (default: 3)
        'limit' => 3,
    ],
```

Another option is to change it directly in the `resources/views/partials/prices.blade.php` file.  

At the top of this file, you will find a call to `Lss\Services\ProductsGetter::get()`. This provides a collection of products, and you can change it for a custom query, for example:

```php
    $products = Lss\Models\Product::whereIn('id', [1, 2, 3])->get();
```

Take into account that this file is used in the landing page, but also in the dashboard when forcing user to choose a product or subscription.

##### Other options related with pricing

Inside the `config/shipper.php` file, you will also find other options related to the pricing table, such as `highlighted_product_ids` and `show_available_units`.

```php
    'highlighted_product_ids' => [2],

    'show_available_units' => true,
```

### 6. Other Options

Also, in `config/shipper.php`, you can customize whether you want to allow promotion codes during checkout, trial days on subscription, and the yearly discount applied to subscriptions when the yearly period is chosen.

```php
    'payments' => [
        // Promotions code are created in Stripe
        'allow_promotion_codes' => env('SHIPPER_ALLOW_PROMO_CODES', true),

        // Trials days only apply to subscriptions
        'trial_days' => env('SHIPPER_TRIAL_DAYS', 7),

        // Discount for yearly subcriptions only
        'yearly_discount' => env('SHIPPER_YEARLY_DISCOUNT', 20),
    ],
```
