# Commander
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)
[![Build Status](https://travis-ci.org/robotusers/commander.svg?branch=master)](https://travis-ci.org/robotusers/commander)
[![codecov](https://codecov.io/gh/robotusers/commander/branch/master/graph/badge.svg)](https://codecov.io/gh/robotusers/commander)

Command Bus abstraction for PHP.

## Installation

```
composer require robotusers/commander
```

## Command Bus abstraction

This library provides a `CommandBusInterface` which a command bus should implement.

## Using the command bus

```php
use Robotusers\Commander\CommandBusAwareInterface;
use Robotusers\Commander\CommandBusAwareTrait;

class OrdersController implements CommandBusAwareInterface
{
   use CommandBusAwareTrait;

   public function makeOrder()
   {
        ...
        $command = new MakeOrderCommand($data);
        $this->getCommandBus()->handle($command);
        ...
   }

}
```

## Adapters

The library provides adapters for the most common command bus implementations.

### Tactician

```
composer require league/tactician
```

```php
use League\Tactician\CommandBus;
use Robotusers\Commander\Adapter\TacticianAdapter;

$commandBus = new CommandBus($middleware);
$adapter = new TacticianAdapter($commandBus);

$controller->setCommandBus($adapter);
```

### SimpleBus/MessageBus

```
composer require simple-bus/message-bus
```

```php
use Robotusers\Commander\Adapter\SimpleBusAdapter;
use SimpleBus\Message\Bus\Middleware\MessageBusSupportingMiddleware;

$commandBus = new MessageBusSupportingMiddleware();
$adapter = new SimpleBusAdapter($commandBus);

$controller->setCommandBus($adapter);
```

### PSB - ProophServiceBus

```
composer require prooph/service-bus
```

```php
use Prooph\ServiceBus\CommandBus;
use Robotusers\Commander\Adapter\ServiceBusAdapter;

$commandBus = new CommandBus();
$adapter = new ServiceBusAdapter($commandBus);

$controller->setCommandBus($adapter);
```

### Writing a custom adapter

You can write your custom adapter. The adapter must implement `Robotusers\Commander\CommandBusInterface`.

```php
class MyAdapter implements CommandBusInterface
{
    public function handle($command)
    {
        //handle a command
    }
}
```