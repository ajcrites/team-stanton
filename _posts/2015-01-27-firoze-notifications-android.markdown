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

Throughout this post, you will notice `this` being used as a parameter a lot. It refers to the `context`.

The following snippet will show how to trigger a simple notification using the three items from the previous section.

{% highlight java %}
NotificationCompat.Builder myBuilder = new NotificationCompat.Builder(this);
myBuilder.setSmallIcon(R.drawable.my_small_icon);
myBuilder.setContentTitle(myTitleString);
myBuilder.setContentText(myContentString);
// ^ these calls can be chained, if you like

Notification myNotification = myBuilder.build();

NotificationManager myNotificationManager = (NotificationManager) this.getSystemService(Context.NOTIFICATION_SERVICE);

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

#### Other Notification Options/Flags

Perhaps you want to capture the user's attention via sound, or maybe vibrate the device, or even light up the device's notification LED. These can be set via flags offered by the `Notification` class, which are then set by calling:

{% highlight java %}
myNotification.defaults |= <your_desired_flag>;
{% endhighlight %}

Let's look at the default flags available:

{% highlight java %}
// sound
myNotification.defaults |= Notification.DEFAULT_SOUND;

// vibrate device
myNotification.defaults |= Notification.DEFAULT_VIBRATE;

// LED
myNotification.defaults |= Notification.DEFAULT_LIGHTS;

// combo platter
myNotification.defaults |= Notification.DEFAULT_ALL;
{% endhighlight %}

#### Handling Notification Taps

So far, the notification that we have set up, will not do anything if tapped. To set an action for the notification, you should set a [`PendingIntent`](http://developer.android.com/reference/android/app/PendingIntent.html) which in turn contains an `Intent` to launch a particular activity of your choosing. You can also use this object to set any data associated with the notification to send to the resulting activity. Then, call the `.setContentIntent()` of your `NotificationBuilder` object, passing it the `PendingIntent` object.

{% highlight java %}
Intent resultIntent = new Intent(this, MyActivity.class);

PendingIntent pendingIntent = PendingIntent.getActivity(this, 123, resultIntent, PendingIntent.FLAG_CANCEL_CURRENT);
// to keep track of the pendingIntent you created, preserve the '123' or replace with your own id (for example, if this involves data, you can use the id of that object)

myBuilder.setContentIntent(pendingIntent);
{% endhighlight %}

#### Dismissing a Notification

If you would like your notification to be dismissed when the user taps on it. This can be done by setting another flag. For example:

{% highlight java %}
myNotification.flags |= Notification.FLAG_AUTO_CANCEL;
{% endhighlight %}

The above step assumes that you have already set a `PendingIntent` for the notification. Otherwise, the only way to dismiss the notification is to swipe it.

{:.screenshot}
![](http://i.imgur.com/Q0vxGFH.gif)


# Thank You!

I appreciate your patience in reading through this post. Coming up next: delayed/interval-based notifications!
