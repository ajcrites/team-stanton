---
layout: post
title:  "Notifications in Android"
date:   2015-01-27 13:41:00
category: android
author: firoze
---

Code will be available as a part of [this repository](https://github.com/firoze/AndroidPlayground).

## Sometimes, you really just want to bug a user

Displaying notifications in an Android app is one of the most common tasks you do when building your app.  
The SDK makes it easy for us to create and display notifications, and offers a lot of flexibility as to:  

- the kind of notification to display,
- whether it will be shown immediately, delayed or interval-based
- additional options
  - sound
  - vibration
  - notification LED (if the device supports it)

# Requirements
The basic requirements for a notification are:

- A small icon
- Text for the title
- Text for the content

If any one of these are missing, there won't be an error thrown, but the notification might not be displayed when triggered.

# Basic Notification

The following snippet will show how to trigger a simple notification using the three items from the previous section.

{% highlight java %}
NotificationCompat.Builder myBuilder = new NotificationCompat.Builder(this); // 'this' is the context
myBuilder.setSmallIcon(R.drawable.my_small_icon);
myBuilder.setContentTitle(myTitleString);
myBuilder.setContentText(myContentString);
// ^ these calls can be chained, if you like

Notification myNotification = myBuilder.build();

NotificationManager myNotificationManager = (NotificationManager) this.getSystemService(Context.NOTIFICATION_SERVICE);  // 'this' is the context

// the '111' can be any random number of your choosing
// but preserve this if you want to update the notification later on
myNotificationManager.notify(111, myNotification);
{% endhighlight %}

{:.screenshot}
![](http://i.imgur.com/XmBZWtp.png)
![](http://i.imgur.com/0UOJk3v.png)

As you can see, the notification is displayed using the small notification icon, but when you expand the notification panel, a white tile is displayed. This can be remedied by calling the `.setLargeIcon()` method in your `NotificationCompat.Builder` instance. For example:

{% highlight java %}
// .setLargeIcon expects a bitmap
myBuilder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.drawable.my_large_icon));
{% endhighlight %}

And now, the notification will no longer have a generic white tile.

{:.screenshot}
![](http://i.imgur.com/Iwa0PbF.png)

# Customizing the Notification

#### Long Content Text
If you have a really large amount of text to display on the notification, then you can take advantage of "expandable notifications" (introduced in Android 4.1) then you can use the builder's `setStyle()` method, and pass it an instance of `NotificationCompat.BigTextStyle` and set your long text using its own `.bigText()` method. For example:

{% highlight java %}
myBuilder.setStyle(new NotificationCompat.BigTextStyle().bigText(myLongText));
{% endhighlight %}

And now, the content text will change as you expand the notification.

{:.screenshot}
![](http://i.imgur.com/FUeQcxq.gif)
