## Landing Page

**Table Of Contents:**

1. [Update landing page](/doc/landing-page.md#update-landing-page)
2. [Capture emails of interested users](/doc/landing-page.md#capture-emails-of-interested-users)
3. [Change Logos](/doc/landing-page.md#update-landing-page)
4. [Update FAQ](/doc/landing-page.md#change-logs)

### Update landing page

The landing page is a blade file under `resources/views/welcome.blade.php`.

There you can find everything on it and make any change you want. We are using some partial blade views to show testimonials, pricing, and other sections. Fill free to update them as you need.

### Capture emails of interested users

If you app is not ready and you don't want users to sign up yet you can start capturing emails.

You can do this just by adding the next to your `.env` file:

```env
LSS_LANDING_RELEASED=false
```

Also, you can change it in `config/shipper.php` under the `landing.released` option.

### Change Logos
### Update FAQ
