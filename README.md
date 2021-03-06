**Experimental: do not use yet in your production environment**

# Schedule artisan commands to run at a sub-minute frequency

[![Latest Version on Packagist](https://img.shields.io/packagist/v/spatie/laravel-short-schedule.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-short-schedule)
[![GitHub Tests Action Status](https://img.shields.io/github/workflow/status/spatie/laravel-short-schedule/run-tests?label=tests)](https://github.com/spatie/laravel-short-schedule/actions?query=workflow%3Arun-tests+branch%3Amaster)
[![Total Downloads](https://img.shields.io/packagist/dt/spatie/laravel-short-schedule.svg?style=flat-square)](https://packagist.org/packages/spatie/laravel-short-schedule)

[Laravel's native scheduler](https://laravel.com/docs/master/scheduling) allows you to schedule Artisan commands to run every minute. 

If you need to execute something with a higher frequency, for example every second, than you've come to the right package. With laravel-short-schedule installed, you can do this

```php
// in app\Console\Kernel.php

protected function shortSchedule(\Spatie\ShortSchedule\ShortSchedule $shortSchedule)
{
    // this command will run every second
    $shortSchedule->command('artisan-command')->everySecond();
    
    // this command will run every 30 seconds
    $shortSchedule->command('another-artisan-command')->everySeconds(30);
}
```

## Support us

Learn how to create a package like this one, by watching our premium video course:

[![Laravel Package training](https://spatie.be/github/package-training.jpg)](https://laravelpackage.training)

We invest a lot of resources into creating [best in class open source packages](https://spatie.be/open-source). You can support us by [buying one of our paid products](https://spatie.be/open-source/support-us).

We highly appreciate you sending us a postcard from your hometown, mentioning which of our package(s) you are using. You'll find our address on [our contact page](https://spatie.be/about-us). We publish all received postcards on [our virtual postcard wall](https://spatie.be/open-source/postcards).

## Installation

You can install the package via composer:

```bash
composer require spatie/laravel-short-schedule
```

In your production environment you can start the short scheduler with this command

```bash
php artisan short-schedule:run
```

You should use a process monitor like [Supervisor](http://supervisord.org/index.html) to keep this task going at all times, and to automatically start it when your server boots. Whenever you change the schedule, you should restart this command.

## Usage

In `app\Console\Kernel` you should add a method named `shortSchedule`.

```php
// in app\Console\Kernel.php

protected function shortSchedule(\Spatie\ShortSchedule\ShortSchedule $shortSchedule)
{
    // this artisan command will run every second
    $shortSchedule->command('artisan-command')->everySecond();
}
```

### Specify the amount of seconds

You an artisan command every single second like this:

```php
$shortSchedule->command('artisan-command')->everySecond();
```

You can specify a specific amount of seconds using `everySeconds`

```php
$shortSchedule->command('artisan-command')->everySeconds(30);
```

 ### Scheduling shell commands
 
 Use `exec` to schedule a bash command.
 
```php
$shortSchedule->bash('bash-command')->everySecond();
```
 
 ### Preventing overlaps
 
 By default, a scheduled command will run, even if the previous invocation is still running.
 
 You can prevent that by tacking on `withoutOverlapping`
 
```php
$shortSchedule->command('artisan-command')->everySecond()->withoutOverlapping();
```
 
 ### Between time constraints
 
 Limit the task to run between start and end times.
 
 ```php
 $shortSchedule->command('artisan-command')->between('09:00', '17:00')->everySecond();
 ```

It is safe use overflow days. In this example the command will run on every second between 21:00 and 01:00

 ```php
 $shortSchedule->command('artisan-command')->between('21:00', '01:00')->everySecond();
 ```
 
 ### Truth test constraints
 
 The command will run if the given closure return a truthy value. The closure will be evaluated at the same frequency the command is scheduled. So if you schedule the command to run every second, the given closure will also run every second.
 
```php
$shortSchedule->command('artisan-command')->when(fn() => rand() %2)->everySecond();
```

 ### Environment constraints
 
 The command will only run on the given environment.
 
 ```php
 $shortSchedule->command('artisan-command')->environment('production')->everySecond();
 ```

You can also pass an array:

 ```php
 $shortSchedule->command('artisan-command')->environment(['staging', 'production'])->everySecond();
 ```

### Composite constraints

You can use all constraints mentioned above at once. The command will only execute if all the used constraints pass.

 ```php
 $shortSchedule
   ->command('artisan-command')
   ->between('09:00', '17:00')
   ->when($callable)
   ->everySecond();
 ```

## Events

TODO:

## Testing

``` bash
composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Security

If you discover any security related issues, please email freek@spatie.be instead of using the issue tracker.

## Credits

- [Freek Van der Herten](https://github.com/freekmurze)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
