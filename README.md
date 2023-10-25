# ShipSaaS Logger - Laravel Unique Request ID Logger

[![Build & Test (PHP 8.2)](https://github.com/shipsaas/shipsaas-logger/actions/workflows/build.yml/badge.svg)](https://github.com/shipsaas/shipsaas-logger/actions/workflows/build.yml)
[![codecov](https://codecov.io/gh/shipsaas/shipsaas-logger/graph/badge.svg?token=1cWfW8UeUh)](https://codecov.io/gh/shipsaas/shipsaas-logger)


Laravel ShipSaasLogger enables the tracing of requests across servers
by marking each request with a unique ID 🆔 for every log record of the given request.

Skyrocket your production debugging ⚒️.

Additionally, ShipSaasLogger solves the **missing logs** problem when you have a huge amount of traffic 😎. 
Making production logs more reliable and engineers won't have to scream "I can't find the logs" 🔥.

![ShipSaaS Logger](.github%2Fshipsaas-logger.png)

## Supports
- Laravel 10+
- PHP 8.2+

## Installation

Install the library:

```bash
composer require shipsaas/shipsaas-logger
```

## Usage

We ship a new Logger driver called `shipsaas-logger` which handles:

- Create log file based on the requestId, e.g.: `7a559daf-f1fe-4a97-8eb8-40d0907c986b.log`
- Write request-based logs there
- A fallback to the default log file, if `requestId` is not presented

Thus, it fixes the missing logs issue because we have **independent log files** and not yolo-write into 1 file (which will mess up the logs when having high traffic) 🚀

And the last step, tell Sumologic (or Cloudwatch, etc.) to sync your logs folder 🔥. All your logs will be on the cloud.

### Set up `config/logging.php`

Add a new channel called `shipsaas-logger` and change the configuration based on your needs

```php
'shipsaas-logger' => [
    'driver' => 'shipsaas-logger',
    'path' => storage_path('logs/requests/laravel.log'), // can change to your desired path
    'default_log_file' => storage_path('logs/laravel.log'), // can change to your desired path
    'id-type' => 'ulid', // uuid, orderedUuid, ulid
    'use_json_format' => false, // set to true to write log as JSON
],
```

### Update .env

Change the `LOG_CHANNEL` to `shipsaas-logger` 

```dotenv
LOG_CHANNEL=shipsaas-logger
```

### Play
Now that you have everything, hit some requests and try it out 😎.

And congrats, no more "missing logs" pain for your app 😉.

## Usage (Minimalism)

### Inject Unique Request Id Logger Processor

By simply putting this piece of code into your `AppServiceProvider`:

```php
// AppServiceProvider.php

use ShipSaasUniqueRequestLogger\UniqueRequestIdLoggerInitiator;

public function boot(): void
{
    $this->app->booted(fn () => UniqueRequestIdLoggerInitiator::init());
}
```

### Play

Now that you have injected ShipSaaS Logger, hit some requests and try it out 😎.

Note: The Minimalism way only injects the UniqueRequestID generation into your application, it **won't have any** improvement for missing logs issue.

## Testing

Run `composer test` 😆

## Contributors
- Seth Phat

## Contributions & Support the Project

Feel free to submit any PR, please follow PSR-1/PSR-12 coding conventions and testing is a must.

If this package is helpful, please give it a ⭐️⭐️⭐️. Thank you!

## License
MIT License
