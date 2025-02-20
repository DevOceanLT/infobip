# Laravel Notifications Channel for Infobip

[![Latest Version on Packagist](https://img.shields.io/packagist/v/devoceanlt/infobip.svg?style=flat-square)](https://packagist.org/packages/devoceanlt/infobip)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/devoceanlt/infobip/master.svg?style=flat-square)](https://travis-ci.org/devoceanlt/infobip)
[![StyleCI](https://styleci.io/repos/285692286/shield)](https://styleci.io/repos/285692286)
[![Quality Score](https://img.shields.io/scrutinizer/g/devoceanlt/infobip.svg?style=flat-square)](https://scrutinizer-ci.com/g/devoceanlt/infobip)
[![Code Coverage](https://img.shields.io/scrutinizer/coverage/g/devoceanlt/infobip/master.svg?style=flat-square)](https://scrutinizer-ci.com/g/devoceanlt/infobip/?branch=master)
[![Total Downloads](https://img.shields.io/packagist/dt/devoceanlt/infobip.svg?style=flat-square)](https://packagist.org/packages/devoceanlt/infobip)

This package makes it easy to send notifications using [Infobip](https://www.infobip.com/) with Laravel 5.5+, 6.x and 7.x

## Contents

- [Installation](#installation)
	- [Setting up the Infobip service](#setting-up-your-infobip-account)
- [Usage](#usage)
	- [On-Demand Notifications](#on-demand-notifications)
    - [Available Message methods](#available-message-methods)
- [Changelog](#changelog)
- [Testing](#testing)
- [Security](#security)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)


## Installation

You can install the package via composer:

``` bash
composer require devoceanlt/infobip
```

## Setting up your Infobip account

Add your Infobip Product Token and default originator (name or number of sender) to your `config/services.php`:

```php
// config/services.php
...
'infobip' => [
    'username' => env('INFOBIP_USERNAME'),
    'password' => env('INFOBIP_PASSWORD'),
    'from' => env('INFOBIP_FROM', 'Info'),
    'notify_url' = env('INFOBIP_NOTIFY_URL', null),
],
...
```

To change `Base URL` to personal use this ([See more](https://dev.infobip.com/getting-started/base-url))

```php
...
'infobip' => [
    ...
    'baseUrl' => env('INFOBIP_BASE_URL', null),
],
...
```

## Usage

You can use the channel in your via() method inside the notification:

```php
use Illuminate\Notifications\Notification;
use NotificationChannels\Infobip\InfobipMessage;

class AccountApproved extends Notification
{
    public function via($notifiable)
    {
        return ["infobip"];
    }

    public function toInfobip($notifiable)
    {
        return (new InfobipMessage)->content("Your account was approved!");
    }
}
```

In your notifiable model, make sure to include a routeNotificationForInfobip() method, which returns a phone number or an array of phone numbers.

```php
public function routeNotificationForInfobip()
{
    return $this->phone_number;
}
```

### On-Demand Notifications
Sometimes you may need to send a notification to someone who is not stored as a "user" of your application. Using the Notification::route method, you may specify ad-hoc notification routing information before sending the notification:

```php
Notification::route('infobip', '5555555555')
            ->notify(new InvoicePaid($invoice));
```

### Available Message methods
`from('')`: Accepts a phone number/sender name to use as the notification sender.. *Make sure to register the sender name at you Infobip dashboard.*

`content('')`: Accepts a string value for the notification body.


## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

``` bash
$ composer test
```

## Security

If you discover any security related issues, please email cliff@nyumbanitechnologies.com instead of using the issue tracker.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

- [Thomson Maguru](https://github.com/tomsgad)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
