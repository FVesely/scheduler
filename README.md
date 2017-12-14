# Scheduler

Small lib for executing php callbacks using cron expression.

## Installation

Install via composer.

```
composer require tlapnet/scheduler
```

Register extension.

```yaml
extensions:
	scheduler: Tlapnet\Scheduler\DI\SchedulerExtension
```

Set-up crontab. Use `scheduler:run` command.

```
* * * * * php path-to-project/console scheduler:run
```

Optionally set temp path for lock files.

```yaml
scheduler:
	path: '%tempDir%/scheduler'
```

## Jobs

### Callback job

```yaml
scheduler:
	jobs:
		- {cron: '* * * * *', callback: App\Model\Pirate::arrgghh}
		- {cron: '*/2 * * * *', callback: App\Model\Parrot::echo}
```

### Custom job

Use `IJob` interface.

```php

class MyAwesomeJob implements IJob
{

	/**
	 * @param DateTime $dateTime
	 * @return bool
	 */
	public function isDue(DateTime $dateTime)
	{
		return TRUE; // When is job ready to run
	}

	/**
	 * @return void
	 */
	public function run()
	{
		// Do something
	}

}

```

And register it.

```yaml
scheduler:
	jobs:
		- App\Model\MyAwesomeJob
```

## Commands

### List

List all jobs.

```
scheduler:list
```

### Run

Run all due jobs.

```
scheduler:run
```
