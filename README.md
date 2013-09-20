mailjet-api-php
=========

`mailjet-api-php` is a PHP library for quick and simple consuming of Mailjet API.

It supports both [RESTful](http://www.mailjet.com/docs/api) and [Event Tracking](https://www.mailjet.com/docs/event_tracking) APIs.

[![Build Status](https://secure.travis-ci.org/KnpLabs/mailjet-api-php.png?branch=master)](http://travis-ci.org/KnpLabs/mailjet-api-php) [![SensioLabsInsight](https://insight.sensiolabs.com/projects/b7bb0aa1-402e-47d4-93d0-d957980000b3/mini.png)](https://insight.sensiolabs.com/projects/b7bb0aa1-402e-47d4-93d0-d957980000b3) [![Scrutinizer Quality Score](https://scrutinizer-ci.com/g/KnpLabs/mailjet-api-php/badges/quality-score.png?s=46285297cb5c9acface8efbe96991f622923374f)](https://scrutinizer-ci.com/g/KnpLabs/mailjet-api-php/) [![Latest Stable Version](https://poser.pugx.org/knplabs/mailjet-api-php/version.png)](https://packagist.org/packages/knplabs/mailjet-api-php) [![Composer Downloads](https://poser.pugx.org/knplabs/mailjet-api-php/d/total.png)](https://packagist.org/packages/knplabs/mailjet-api-php)

## Usage

### RESTful API

To ease consumption of RESTful API there's a [RequestApi](src/Mailjet/Api/RequestApi.php) helper class, which lists all available queries.
Check [Mailjet documentation](http://www.mailjet.com/docs/api) for a detailed list of queries.

Simple example:

```php
<?php

// This file is generated by Composer
require_once 'vendor/autoload.php';

use Mailjet/Api/Client;
use Mailjet/Api/RequestApi;

$client = new Client('api_key', 'secret_key');
$userInfo = $client->get(RequestApi::USER_INFOS);

var_dump($userInfo);

//(
//    [username] => KnpLabs
//    [email] => hello@Knplabs.com
//    [locale] => en_US
//    [currency] => USD
//    [timezone] => America/New_York
//    [firstname] => KnpLabs
//    [lastname] =>
//    [company_name] => KnpLabs
//    [contact_phone] =>
//    [address_street] =>
//    [address_postal_code] =>
//    [address_city] =>
//    [address_country] =>
//    [tracking_openers] => 1
//    [tracking_clicks] => 1
//)
```

Sending a POST request:

```php
<?php

// ... initialize client

$result = $client->post(RequestApi::USER_DOMAIN_ADD, array(
    'domain' => 'example.com'
));

var_dump($result);

//(
//    [status]    => 200
//    [file_name] => empty_file
//    [id]        => 123
//)
```
### Event Tracking API

`mailjet-api-php` provides a clean OOP interface to interact with [Event Tracking API](https://www.mailjet.com/docs/event_tracking).

```php
<?php

use Mailjet/Event/EventFactory;

$factory = new EventFactory();

// fetch incoming data into $data array, example
//(
//    [event]          => open
//    [email]          => hello@Knplabs.com
//    [mj_contact_id]  => 123
//    [mj_campaign_id] => 123
//    [customcampaign] => Hello from KnpLabs
//    [ip]             => 127.0.0.1
//    [geo]            => US
//    [agent]          => Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:21.0) Gecko/20100101 Firefox/21.0
//)

$event = $factory->createEvent($data);

echo get_class($event); // \Mailjet\Event\Events\OpenEvent
echo $event->getType(); // open
echo $event->getIp();   // 127.0.0.1
```

> Note: this library is not responsible for validation of incoming data.
> However, assuming the data is coming from Mailjet servers, it will work correctly.

### Symfony2 integration

You don't need a special bundle to use the RESTful API with Symfony2, you can initialize the service with a simple config:

```yaml
services:
    knp_mailjet.api:
        class: Mailjet\Api\Client
        arguments: [<your_mailjet_api_key>, <your_mailjet_secret_key>]
```

And that's it, Mailjet RESTful API is now available via:

```php
<?php

$this->container->get('knp_mailjet.api');
```

However, if you need both RESTful and Event API support, then there's [KnpMailjetBundle](https://github.com/KnpLabs/KnpMailjetBundle).

## Installation

The first step to use `mailjet-api-php` is to download Composer:

```bash
$ curl -s http://getcomposer.org/installer | php
```

Now add `mailjet-api-php` with Composer:

```bash
$ php composer.phar require knplabs/mailjet-api-php:*
```

And that's it! Composer will automatically handle the rest.

Alternatively, you can manually add the dependency to `composer.json` file...

```json
{
    "require": {
        "knplabs/mailjet-api-php": "~1.0"
    }
}
```

... and then install our dependencies using:
```bash
$ php composer.phar install
```
## Requirements

* PHP >= 5.3.8
* HTTP component of [Guzzle](http://guzzlephp.org/) library
* (optional) Symfony2 Debug Component

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) file.

## Running the Tests

To run unit tests, you'll need a set of dev dependencies you can install using Composer:

```
php composer.phar install --dev
```

Once installed, just launch the following command:

```
phpunit
```

## Credits

#### Sponsored by

[![KnpLabs Team](http://knplabs.pl/bundles/knpcorporate/images/logo.png)](http://knplabs.com)

## License

mailjet-api-php is released under the MIT License. See the bundled [LICENSE](LICENSE) file for
details.
