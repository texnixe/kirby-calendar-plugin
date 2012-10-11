kirby-calendar-plugin
=====================

A plugin for the [Kirby CMS](http://getkirby.com) to easily implement a event calendar.

## Installation

All you have to do is to put the `calendar.php` in the `/site/plugins` directory.

## Usage

### YAML input

The events shown by the Calendar Plugin will be read out of a field of the page, structured like this in its source `.txt`:

```yaml
Events:
01.01.2012 10:00 -> 02.01.2012 10:00:
	Location: The Pub
	Price: free
	
03.01.2012 -> 04.01.2012:
	Location: Concert hall
	Price: a beer

05.01.2012:
	Description: chillin'
	Location: couch
	Price: priceless
```

More general each event looks like this (`[]` is optional):

```yaml
beginDate [beginTime] [-> endDate [endTime]]:
	[Category: [Value]]
	[...as much categorys as you like]
```

You can use different formatting for the date and time (e.g. `01-31-2012 11pm`). If your preferred formatting is not supported, check the `'timezone'` option (see Options below).

See [Structured Field Content](http://getkirby.com/blog/structured-field-content) for more Information about YAML and Kirby.

### The page template

To include the calendar into your website you have to put the following code in the content section of your template:

```php
<?php calendar(yaml($page->events()), $options, 'table'); ?>
```

`$page->events()` refers to the field of the page containing your events. If you have called it `Foo:`, you have to use `$page->foo()`.

The second and third parameters of `calendar()` are both optional. `$options` is the array of options (see below) and `'table'` is the name of the calendar template (not yet implemented, see The calendar tempalte below).

### Options

The options are set in an array. The available options are:

#### lang

`lang` sets the locale for the time formatting (e.g. the names of the months). It must be a valid **RFC 1766** or **ISO 639** code. For example `de_DE`.

Default is `en_US`.

#### timezone

`timezone` is important for the date formats the calendar is able to read from the input. By default it should be able to read most of the common formats but if you encounter an error check this option. All valid timezones are listed [here](http://php.net/manual/en/timezones.php).

Default is the timezone of your server.

#### dateFormat

`dateFormat` sets the format of the date and time displayed for each event. For example `%d.%m.` will result in `31.12.`. All formatting characters are listed [here](http://php.net/manual/en/function.strftime.php).

Default is `%d-%m-%Y`.

#### monthFormat

`monthFormat` sets the format of the date which divides the calendar whenever a new month begins. For example `%B %Y` will result in `December 2012`. The allowed formatting characters are the same as at `dateFormat`.

Default is `%B`.

#### hasTime

If you set the `hasTime` flag to `false` the calendar will assume that you never specify a time for the events. By default at a date without time, the time will be set to `0:00`(`0am`). With this option set `false` all times are calculated `+23:59` so that the past events are marked properly.

Default is `true`.

In a future version this will be done automatically for each event.

#### noEntryMsg

This option is for multi language support. Here you can set the message that will be shown if no event is available.

Default is `No entry.`.

#### Example

```php
<?php $options = array(
	'lang' 		=> (c::get('lang.current') === 'de') ? 'de_DE' : 'fr_FR',
	'timezone' 	=> 'Europe/Berlin',
	'dateForm'	=> '%d.',
	'monthForm'	=> '%B %Y',
	'hasTime'	=> false
);?>
```

### The calendar template

In a future version you will be able to specify the layout of the calendar in a separate template file. The only layout available is `table`.
