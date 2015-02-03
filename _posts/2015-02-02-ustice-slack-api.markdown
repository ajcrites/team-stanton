---
layout: post
title:  "Slack API javascript library"
date:   2015-02-02 11:00:00
category: javascript
author: ustice
---
![](https://slack.global.ssl.fastly.net/22690/img/landing_slack_hash_wordmark_logo.png)

If you haven't heard already, [slack](https://slack.com) is a [fast-growing](http://www.businessinsider.com/andreessen-slack-has-gone-crazy-viral-2014-2) communications platform for organizations. It's basically [IRC](http://en.wikipedia.org/wiki/Internet_Relay_Chat) with super powers, and let's you integrate with all kinds of services out of the box.

But what if you want to integrate it into some other service or project of your own? Luckily, they have a great [api](https://api.slack.com/) for that, but I found it a bit tedious to use HTTPS requests and doing all of the error-handling for each of them. I looked for a good javascript library for slack, but there just wasn't one that was complete.

[So I wrote my own](https://github.com/Ustice/slack-api).

### slack-api

`slack-api` is written to mirror the slack web API, and is structured exactly like the web API. It comes with two interfaces: Node-style, and Promise-based.

#Installation

`slack-api` is published on [npm](https://www.npmjs.com/package/slack-api), so it's super easy to install. In your Node project directory, just simply type:

{% highlight bash%}
npm install --save slack-api
{% endhighlight %}

## Using slack-api

To use it, you simply follow the standard Node-style as seen below:

{% highlight javascript %}
'use strict';

var slack = require('slack-api');

try {
  slack.api.test({}, function (error, data) {
    if (error) {
      console.error('Slack didn\'t like what you were looking for. ', error);
    }
    console.log('Here\'s what Slack returned. ', data);
  });
} catch (error) {
  console.log('I encountered an error while communicating with Slack. ', error);
}
{% endhighlight %}

Alternately, you can use the Promises-style methods:

{% highlight javascript %}
'use strict';

var Slack = require('slack-api').promisify();

Slack.api.test({}).then(function (data) {
  console.log(data);
}).catch(Slack.errors.SlackError, function (error) {
  console.log('Slack did not like what you did: ' + error.message);
}).catch(Slack.errors.CommunicationError, function (error) {
  console.error('Error communicating with Slack. ' + error.message);
}).catch(Slack.errors.SlackServiceError, function (error) {
  console.error('Error communicating with Slack. ' + error.message);
  //To get error details
  console.error(error.errorDetails);
});
{% endhighlight %}

## Opportunities for future development

There are currently a few things that I can see for future development on this project. The first is that the Slack web API is not a real REST API, and it takes both `GET` and `POST` requests for everything. It's developed so that it uses a single config file, and very few extra functions.

# Refactor config files

One thing that would make it easier to develop for would be to refactor the config file so that each section gets its own file. This should be fairly easy to change.

# Fix files.upload method

There is also currently a bug with how the `files.upload` method. Since it is using the same method as the rest of the API, it doesn't actually work. Slack provides two ways to upload files: multi-part forms and as `POST` data. This shouldn't be too difficult to implement, but it will require that the function that builds the final API not override this custom method. Also, it would be good to make sure that that happens in general.

# Expand unit-test coverage

Currently, there are only a few unit tests. This was mostly a time issue. It would be ideal to expand this to the entire API. This would be a great project for someone that is new to writing unit tests. It would also allow us to make sure that everything is working as it should as well as test that Slack is responding as they report (I have seen a few rare cases where this doesn't happen).

# Add CI to the project

It would be really nice if when an update is made to the repo, that it will automatically bump the version number and post it to npm. Currently, this is a manual process, and while it is not a big hinderance, it would make future development easier.

## Wrapping up

I hope that you find this library useful, either a tool for accessing such a great and useful service, or as a teaching too to see how you can keep your code as [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) as possible.

Working with APIs may not be particularly sexy, but there is no need for it to be painful. If there is an API that you use a lot, instead of accessing it directly, create a wrapper, and call that. If you think that your wrapper is particularly useful, please share. After all the best way to stay DRY is by reusing code that someone else has written and fixed most of the issues with.
