---
layout: post
title:  "Pickers in Android"
date:   2015-01-30 13:11:00
category: android
author: firoze
---

Code will be available as a part of [this repository](https://github.com/firoze/AndroidPlayground).

Throughout this post, you will notice `this` being used as a parameter a lot. It refers to the `context`.

## Do you want to pick stuff?

More often than not, you will find yourself in need of a "picker". The Android SDK has made the most common ones available to us: date and time pickers. This post will take us through some snippets in getting those set up, take a look at some libraries which offer some more advanced pickers (or rather, fancier versions of the same pickers), and also take a look at how you can build your own picker, for your own needs.

## I want a picker now, though!

{% highlight java %}
TimePickerDialog.OnTimeSetListener myTimePickerCallback = new TimePickerDialog.OnTimeSetListener() {
    @Override
    public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
        // do stuff with the time from the picker
    }
};

new TimePickerDialog(this,
        TimePickerDialog.THEME_TRADITIONAL,
        myTimePickerCallback,
        Calendar.getInstance().get(Calendar.HOUR),
        Calendar.getInstance().get(Calendar.MINUTE),
        false).show();
{% endhighlight %}
