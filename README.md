# clue/reactphp-eventsource

[![CI status](https://github.com/clue/reactphp-eventsource/workflows/CI/badge.svg)](https://github.com/clue/reactphp-eventsource/actions)

Event-driven EventSource client, receiving streaming messages from any HTML5 Server-Sent Events (SSE) server,
built on top of [ReactPHP](https://reactphp.org/).

> Note: This project is in early alpha stage! Feel free to report any issues you encounter.

**Table of contents**

* [Quickstart example](#quickstart-example)
* [Usage](#usage)
    * [EventSource](#eventsource)
* [Install](#install)
* [Tests](#tests)
* [License](#license)

## Quickstart example

Once [installed](#install), you can use the following code to stream messages
from any Server-Sent Events (SSE) server endpoint:

```php
$loop = Factory::create();
$es = new Clue\React\EventSource\EventSource('https://example.com/stream.php', $loop);

$es->on('message', function (Clue\React\EventSource\MessageEvent $message) {
    //$data = json_decode($message->data);
    var_dump($message);
});

$loop->run();
```

See the [examples](examples).

## Usage

### EventSource

The `EventSource` class is responsible for communication with the remote Server-Sent Events (SSE) endpoint.

The `EventSource` object works very similar to the one found in common
web browsers. Unless otherwise noted, it follows the same semantics as defined
under https://html.spec.whatwg.org/multipage/server-sent-events.html

It requires the URL to the remote Server-Sent Events (SSE) endpoint and also
registers everything with the main [`EventLoop`](https://github.com/reactphp/event-loop#usage)
in order to handle async HTTP requests.

```php
$loop = React\EventLoop\Factory::create();

$es = new Clue\React\EventSource\EventSource('https://example.com/stream.php', $loop);
```

If you need custom connector settings (DNS resolution, TLS parameters, timeouts,
proxy servers etc.), you can explicitly pass a custom instance of the
[`ConnectorInterface`](https://github.com/reactphp/socket#connectorinterface)
to the [`Browser`](https://github.com/reactphp/http#browser) instance
and pass it as an additional argument to the `EventSource` like this:

```php
$connector = new React\Socket\Connector($loop, array(
    'dns' => '127.0.0.1',
    'tcp' => array(
        'bindto' => '192.168.10.1:0'
    ),
    'tls' => array(
        'verify_peer' => false,
        'verify_peer_name' => false
    )
));
$browser = new React\Http\Browser($loop, $connector);

$es = new Clue\React\EventSource\EventSource('https://example.com/stream.php', $loop, $browser);
```

## Install

The recommended way to install this library is [through Composer](https://getcomposer.org).
[New to Composer?](https://getcomposer.org/doc/00-intro.md)

This will install the latest supported version:

```bash
$ composer require clue/reactphp-eventsource:dev-master
```

This project aims to run on any platform and thus does not require any PHP
extensions and supports running on legacy PHP 5.4 through current PHP 8+.
It's *highly recommended to use PHP 7+* for this project.

## Tests

To run the test suite, you first need to clone this repo and then install all
dependencies [through Composer](https://getcomposer.org):

```bash
$ composer install
```

To run the test suite, go to the project root and run:

```bash
$ php vendor/bin/phpunit
```

## License

This project is released under the permissive [MIT license](LICENSE).

> Did you know that I offer custom development services and issuing invoices for
  sponsorships of releases and for contributions? Contact me (@clue) for details.
