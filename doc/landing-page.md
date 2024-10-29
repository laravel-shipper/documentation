## Landing Page

**Table Of Contents:**

1. [Update landing page](/doc/landing-page.md#1-update-landing-page)
2. [Capture emails of interested users](/doc/landing-page.md#2-capture-emails-of-interested-users)
3. [Change Logos](/doc/landing-page.md#3-update-landing-page)
4. [Update FAQ](/doc/landing-page.md#4-change-logs)

### 1. Update landing page

The landing page is a blade file under `resources/views/welcome.blade.php`.

There you can find everything on it and make any change you want. We are using some partial blade views to show testimonials, pricing, and other sections. Fill free to update them as you need.

### 2. Capture emails of interested users

If you app is not ready and you don't want users to sign up yet you can start capturing emails.

You can do this just by adding the next to your `.env` file:

```env
LSS_LANDING_RELEASED=false
```

Also, you can change it in `config/shipper.php` under the `landing.released` option.

### 3. Change Logos

All logos are in `config/shipper.php` under the `company_logos` option.

It's basically an array of logos. If you want to not show any logo in the landing page you can just leave this array empty.

```php
  'company_logos' => [],
```

And you can update, or remove any logo by updating or adding a new item to the array.

```php
    'company_logos' => [
        [
            'name' => 'MetricsWave',
            'logo' => 'images/metricswave.png',
            'url' => 'https://metricswave.com',
            'logoDark' => 'images/metricswave_dark.png',
        ],
        // …
    ],
```

Each logo has this four properties:

- `name`: an string used as alt in each image.
- `logo`: path to image, used in light mode.
- `logoDark`: path to image, used in dark mode.
- `url`: to the company or project.

By default, all images are stored in `public/images`.

### 4. Update FAQ