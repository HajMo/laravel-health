---
title: Via Oh Dear
weight: 4
---

In certain scenario's, your application can be in such a bad state, that it can't send any notifications anymore. A possible solution is to let [Oh Dear](https://ohdear.app) monitor your checks, and let that service send notifications for you. 

Using this package, you can register a protected endpoint where [the application health check of Oh Dear](http://ohdear.app/docs/general/application-health-monitoring) can read the latest results of the health checks.

## Adding a health check endpoint to your Laravel app

Oh Dear will send an HTTP request to your application to a specific endpoint to get health check. Your application should respond with JSON containing the result of health checks.

You can add such an endpoint using the spatie/laravel-health package.  To do this, must configure the `ohdear_endpoint_key` in the `health` config file.

You can publish that `health` with this command:

```bash
php artisan vendor:publish --tag="health-config"
```

These are some of the default values in the published `health` config file.

```php
// in app/config/health.php

/*
 * You can let Oh Dear monitor the results of all health checks. This way, you'll
 * get notified of any problems even if your application goes totally down. Via
 * Oh Dear, you can also have access to more advanced notification options.
 */
'oh_dear_endpoint' => [
    'enabled' => false,

    /*
     * When this option is enabled, the checks will run before sending a response.
     * Otherwise, we'll send the results from the last time the checks have run.
     */
    'always_send_fresh_results' => true,

    /*
     * The secret that is displayed at the Application Health settings at Oh Dear.
     */
    'secret' => env('OH_DEAR_HEALTH_CHECK_SECRET'),

    /*
     * The URL that should be configured in the Application health settings at Oh Dear.
     */
    'url' => '/oh-dear-health-check-results',
],
```

To get started:

- set the `enabled` config option to `true`
- add a `secret` (we recommend putting it in the `.env` file, just like you would do for any application secret or password)
- optionally customize the `url` where the health check endpoint will be registered.

## Adding health checks

Now that you have registered an endpoint, let's add some checks to your app. To learn how to do this, head over to the [Registering your first check page in the laravel-health docs](https://spatie.be/docs/laravel-health/v1/basic-usage/registering-your-first-check).

## Configuring the health check at Oh Dear

In the application health check settings screen of a site at Oh Dear, you should fill in the URL and secret that you specified in the `health` config file.

## Avoid stored results and sending notifications locally

Oh Dear will notify you when something goes wrong with one of your health checks, so you can safely disable the notifications sent by the package itself.

You can set the `notifications.enabled` option to `false.

```php
'notifications' => [
    /*
     * Notifications will only get sent if this option is set to `true`.
     */
    'enabled' => true,
```

If you don't want to keep health results in your local application, set the `result_stores` option in the `health` config file to the `InMemoryHealthResultStore`.

```php
// in app/config/health.php

'result_stores' => [
     Spatie\Health\ResultStores\InMemoryHealthResultStore::class,
],
```
