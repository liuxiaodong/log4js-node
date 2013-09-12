# log4js-node [![Build Status](https://secure.travis-ci.org/nomiddlename/log4js-node.png?branch=master)](http://travis-ci.org/nomiddlename/log4js-node)


This was a conversion of the [log4js](http://log4js.berlios.de/index.html)
framework to work with [node](http://nodejs.org). It's changed a lot since then, but there are still plenty of the original parts involved. 

Out of the box it supports the following features:

* coloured console logging
* file appender, with log rolling based on file size or date
* multi-process logging (works fine with node's clusters)
* configurable log message layout/patterns
* different log levels for different log categories (make some parts of your app log as DEBUG, others only ERRORS, etc.)

NOTE: There have been a lot of changes in version 0.7.x, if you're upgrading from an older version, you should read [0.7-changes](http://github.com/nomiddlename/log4js-node/0.7-changes)

## installation

	npm install log4js


## usage

Minimalist version:

    var log4js = require('log4js');
	var logger = log4js.getLogger();
	logger.debug("Some debug messages");

By default, log4js outputs to stdout with the coloured layout (thanks to [masylum](http://github.com/masylum)), so for the above you would see:

	[2010-01-17 11:43:37.987] [DEBUG] default - Some debug messages

See the examples directory for lots of sample setup and usage code.

## API
Log4js exposes two public functions: `configure` and `getLogger`. If
you're writing your own appender, your code will get access to some
internal APIs, see
[writing-appenders](http://github.com/nomiddlename/log4js-node/writing-appenders.md).

### log4js.configure(config)
Configure takes a single argument. If that argument is a string, it is
considered the path to a JSON file containing the configuration
object. If the argument is an object, it must have the following
fields:

* `appenders` (Object) - this should be a map of named appenders to
  their configuration. At least one appender must be defined.
* `categories` (Object) - this should be a map of logger categories to
  their levels and configuration. The "default" logger category must
  be defined, as this is used to route all log events that do not have
  an explicit category defined in the config. Category objects have
  two fields:
      * `level` - (String) the log level for that category: "trace",
        "debug", "info", "warn", "error", "fatal", "off"
      * `appenders` - (Array) the list of appender names to which log
        events for this category should be sent

The default configuration for log4js, the one used if `configure` is
not called, looks like this:

    {
      "appenders": {
          "console": { "type": "console" }
      },
      "categories": {
          "default": { level: "TRACE", appenders: [ "console" ] }
      }
    }

Use of the default configuration can be overridden by setting the
`LOG4JS_CONFIG` environment variable to the location of a JSON
configuration file. log4js will use this file in preference to the
defaults, if `configure` is not called. An example file can be found
in `test/log4js.json`. An example config file with log rolling is in
`test/with-log-rolling.json`. 

### log4js.getLogger([category])

* `category` (String), optional. Category to use for log events
  generated by the Logger.
  
Returns a Logger instance. Unlike in previous versions, log4js
does not hold a reference to Loggers so feel free to use as many as
you like.

### Logger

Loggers provide the following functions:

* `trace`
* `debug`
* `info`
* `warn`
* `error`
* `fatal`

All can take a variable list of arguments which are used to construct
a log event. They work the same way as console.log, so you can pass a
format string with placeholders. e.g.

    logger.debug("number of widgets is %d", widgets);
    

## Appenders

Log4js comes with file appenders included, which can be configured to
roll over based on a time or a file size. Other appenders are
available as separate modules:

* [log4js-gelf](http://github.com/nomiddlename/log4js-gelf)
* [log4js-smtp](http://github.com/nomiddlename/log4js-smtp)
* [log4js-hookio](http://github.com/nomiddlename/log4js-hookio)

There's also
[log4js-connect](http://github.com/nomiddlename/log4s-connect), for
logging http access in connect-based servers, like express.

## Documentation
See the [wiki](https://github.com/nomiddlename/log4js-node/wiki). Improve the [wiki](https://github.com/nomiddlename/log4js-node/wiki), please.

## Contributing
Contributions welcome, but take a look at the [rules](https://github.com/nomiddlename/log4js-node/wiki/Contributing) first.

## License

The original log4js was distributed under the Apache 2.0 License, and so is this. I've tried to
keep the original copyright and author credits in place, except in sections that I have rewritten
extensively.
