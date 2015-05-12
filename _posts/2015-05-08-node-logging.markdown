---
layout: post
title: "Logging and Error Messaging in Node.js"
date: 2015-05-08 14:04:00
category: javascript
author: lbj417
---

![](https://pbmo.files.wordpress.com/2012/02/log.png?w=300&h=245){: .is-centered}

## Logging: "It's better than bad, it's GOOD!"
In my opinion, logging is one of the most important parts of the server development process.  It is typically the first place a server dev will look when debugging, so it's imperative that the logs hold all relevant information (but not *too much* information).

In this blog, I will cover the basics of error handling, logging methods, what information to put in the logs, log rotation, and the delivery of error messages to the client.

#### Error Handling
I was originally going to write a technical blog about error handling in Node.js.  As part of my research, I found [this article](https://www.joyent.com/developers/node/design/errors) by the folks at Joyent titled "Error Handling in Node.js."  It's a great article that covers a lot of the technical stuff I was going to discuss, so I won't repeat it here, but highly recommend checking it out.  Some highlights:

* Differentiating between **operational errors** (e.g. invalid user input, request timeout) and **programmer errors** (e.g. trying to read property of `undefined`, pass a string when an object is expected)
* Using the JavaScript `Error` class can be helpful. Instead of passing an error to your callback as a string:
         callback('something bad happened');
Try using the `Error` class:
        callback(new Error('something bad happened'));
This way, you will have a JavaScript error object and access to all the properties and methods outlined [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error).
* Operational errors are error conditions that your program should be prepared to handle.  Depending on the error and the needs of the project, you can decide which option is best for handling the error. Options include:
  *  Propagate the error to your client
  *  Retry the operation
  *  Log the error and move on
* Programmer errors are bugs and should be eliminated.  But if and when they seep through the cracks, Joyent recommends crashing and restarting your program automatically with a restarter (à la [forever](https://github.com/foreverjs/forever)).

#### Logging Methods
Most of us are familiar with the most basic form of logging in Node: the console. Using `console.log` will simply print a statement to your console. If you are using a shell script to start the server, you can specify a location (a file, for example) to redirect console logs if you wish. But for most projects, you will want something with more options and flexibility.

Node's package manager ([npm](https://www.npmjs.com/)) is home to a beaucoup of logging libraries. Some popular choices are [`winston`](https://github.com/winstonjs/winston) (sponsored by Nodejitsu and my personal favorite), [`morgan`](https://github.com/expressjs/morgan) and [`bunyan`](https://github.com/trentm/node-bunyan) (which I discovered while writing this blog and am very excited to try).

Most of them have the following general design:

* You create a named logger instance and call methods named after the logging levels
* Logging levels typically include `info`, `warn`, and `error` at a minimum (many libraries allow you to create your own custom levels, too)
* Transport options and sometimes multi-transport support (a transport is basically a storage device for your logs - often a file, a database, or simply the console)

#### What information goes in the logs?
Once you have your logging set up, you will need to start thinking about what information to put in the logs.  Striking the right balance between enough and not too much information can be a challenge, and of course the level of detail depends on the needs of your project and the environment.

Some logging levels, such as `debug` or `trace`, are verbose and detailed.  These levels may be appropriate for a development environment, but not staging or production.  [Bunyan](https://github.com/trentm/node-bunyan) claims that it allows you to read debug-level logs on-demand in production without enabling them and without restarting the server, which seems like a pretty awesome feature.

As for the information, again it depends on the needs of your project, but here are some recommendations:

* Timestamp of each log
* The user id of the user who is making the request
* The function which produced the log
* Response from an external API
* Errors (database errors, connection errors, etc.)
* A human-readable message of what went wrong (or right!)
* Any variables that may have contributed to an error situation (**note: unless specifically dictated by your project, do not put any sensitive user information in the logs**)

It may seem like you only need to log error cases, but it can be helpful to log successes too.  This way, if an error is propagated up the stack, you can see how far the request got before it failed and pinpoint the point of failure more accurately.  You can use different transports for successes and errors (for example, `info` and `error` respectively).

#### Log Storage and Rotation
Wherever you store your logs, at some point they will begin to take up too much space on the file system. You can mitigate this by utilizing log levels as mentioned above, but eventually they will need to be "rotated." Log rotation is an automated process that starts logging fresh with an empty log file. This new log file contains a suffix, usually an auto-incremented integer or a date. You can configure log rotation to occur based on an interval (for example, nightly or weekly) or based on a maximum file size.

Some npm logging modules have built-in support for log rotation. The `winston` logging library's file transport allows you to specify a maximum size for a log file, and it will automatically create a new log file when the size is exceeded. Additionally, `winston` provides a daily rotate file transport, which will rotate logs based on a time interval (even as often as every minute).

Most Linux systems have a built-in utility called [`logrotate`](http://linuxconfig.org/logrotate-8-manual-page) which handles log rotation (by default as a daily cron job). The system administrator can configure the behavior of `logrotate` in a configuration file, specifying when to trigger log rotation, how long to keep backups of old log files, and what to do with older files. `logrotate` can automatically compress, remove, or email log files according to rules set in the configuration.

#### Error Messaging
While the server team is interested in specific errors, the client may not be. After logging the error, the server should respond to the client with an appropriate status code and response object. The contents of the response object depend on the needs of the application. For example, if the application is going to be in multiple languages, you may want to utilize localization.

In any case, it is usually desirable for the server to return a human-readable message describing the error. The level of detail is dependent on the project's design. For a very simple example, if the user tries to log in with an incorrect password, a good server response might be:

        {
            "statusCode": 400,
            "message": {
                "error": "The password you entered is incorrect."
            },
            "timestamp": "2015-05-08T17:19:03.324Z"
        }

Alternatively, you could use error codes and hardcode the values into the client codebase:

        {
            "status": 400,
            "message": {
                "password": "ERR1014"
            },
            "timestamp": "2015-05-08T17:19:03.324Z"
        }

where `ERR1014` is a key linked to the error message value, stored client side.

To ensure uniformity, it is a good idea to use a common function to format all server responses.

## Comments?

Add your comments to [this ticket](https://github.com/Mobiquity/team-stanton/issues/7).
