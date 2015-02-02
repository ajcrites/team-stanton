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

The Android SDK includes classes which extend from the [`AlertDialog`](http://developer.android.com/reference/android/app/AlertDialog.html) class, which give us immediate access to a time or date picker in a dialog, and a simple event listener for when the date/time is set.

# Time

To show a simple time picker dialog easily, we can use the [`TimePickerDialog`](http://developer.android.com/reference/android/app/TimePickerDialog.html) class, which implements an event listener for when the user taps "Done" after choosing a time. There are two constructors for a `TimePickerDialog` but the one used in this example accepts the following parameters:

{% highlight java %}
new TimePickerDialog(
  context,
  theme, // this is optional, one of the constants in AlertDialog
  new TimePickerDialog.OnItemSetListener() {...}, // callback for then user selects time
  pickerHour, // hour to show when dialog appears
  pickerMin, // minute to show when dialog appears
  is24hour // AM/PM selector
  );
{% endhighlight %}

A simple example:

{% highlight java %}
TimePickerDialog.OnTimeSetListener myTimePickerCallback = new TimePickerDialog.OnTimeSetListener() {
    @Override
    public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
        // do stuff with the time from the picker
        // the hour and minute are in 24-hour format
    }
};

new TimePickerDialog(this,
        TimePickerDialog.THEME_TRADITIONAL,
        myTimePickerCallback,
        Calendar.getInstance().get(Calendar.HOUR),
        Calendar.getInstance().get(Calendar.MINUTE),
        false).show();
{% endhighlight %}

This results in a picker that looks like:

{:.screenshot}
![](http://i.imgur.com/TO6Xs2x.png)

# Date

Similar to `TimePickerDialog`, there also exists a [`DatePickerDialog`](http://developer.android.com/reference/android/app/DatePickerDialog.html) which allows us to creata simple date picker with an `onDateSet()` method for immediate usage. Have a look at the following example to get an idea of what that looks like.

{% highlight java %}
DatePickerDialog.OnDateSetListener myDatePickerCallback = new DatePickerDialog.OnDateSetListener() {
  @Override
  public void onDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth) {
    // do stuff with date from picker
  }
};

new DatePickerDialog(this,
  DatePickerDialog.THEME_DEVICE_DEFAULT_DARK,
  myDatePickerCallback,
  Calendar.getInstance().get(Calendar.YEAR),
  Calendar.getInstance().get(Calendar.MONTH),
  Calendar.getInstance().get(Calendar.DAY_OF_MONTH)).show();
{% endhighlight %}

This results in a picker that looks like:

{:.screenshot}
![](http://i.imgur.com/hZaZ6XL.png)

## Why not both?

Given the fact that you can create your own dialogs with custom layouts using the [`AlertDialog`](http://developer.android.com/reference/android/app/AlertDialog.html) class, you can create a custom layout XML file (as both [`DatePicker`](http://developer.android.com/reference/android/widget/DatePicker.html) and [`TimePicker`](http://developer.android.com/reference/android/widget/TimePicker.html) are views you can use in XML). The advantage (and disadvantage) to this that it gives you much more control over styling/positioning of the pickers in your custom dialog view.

(You can always use picker views in your main layout as well, there's no need to *always* put it in a dialog)

The following is a very simple example of how you can include both date and time pickers in a single dialog box.

# XML Layout

NOTE: If you aren't interested in having a separate XML file to keep track of (maybe you want to keep the dialog-related code in a single `.java` file) then you can generate this layout using code as well, and use that `View` instead of inflating this XML file.

{% highlight xml %}
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="center_horizontal"
    android:orientation="vertical">

    <DatePicker
        android:id="@+id/multipicker_date"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <TimePicker
        android:id="@+id/multipicker_time"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</LinearLayout>
{% endhighlight %}

# Java

{% highlight java %}
View multiPickerLayout = LayoutInflater.from(this).inflate(R.layout.dialog_pickers, null);
final DatePicker multiPickerDate = (DatePicker) multiPickerLayout.findViewById(R.id.multipicker_date);
final TimePicker multiPickerTime = (TimePicker) multiPickerLayout.findViewById(R.id.multipicker_time);

DialogInterface.OnClickListener dialogButtonListener = new DialogInterface.OnClickListener() {
  @Override
  public void onClick(DialogInterface dialog, int which) {
    switch(which) {
      case DialogInterface.BUTTON_NEGATIVE: {
        // user tapped "cancel"
        dialog.dismiss();
        break;
      }
      case DialogInterface.BUTTON_POSITIVE: {
        // user tapped "set"
        // here, use the "multiPickerDate" and "multiPickerTime" objects to retreive the date/time the user selected
        break;
      }
      default: {
        dialog.dismiss();
        break;
      }
    }
  }
};

AlertDialog.Builder builder = new AlertDialog.Builder(this, AlertDialog.THEME_DEVICE_DEFAULT_LIGHT);
builder.setView(multiPickerLayout);
builder.setPositiveButton("Set", dialogButtonListener);
builder.setNegativeButton("Cancel", dialogButtonListener);
builder.show();
{% endhighlight %}

The resulting dialog will be something like this:

{:.screenshot}
![](http://i.imgur.com/nYFxOk4.png)

...wait a minute. That doesn't exactly look very nice, does it? This is because, in XML, each view is using it's default appearance parameters. For example, to get rid of the "full calendar view" and just return to regular spinning pickers, the following adjustment has to be made to the `<DatePicker />` tag:

{% highlight xml %}
<DatePicker
  android:id="@+id/multipicker_date"
  android:calendarViewShown="false"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content" />

<!--the "calendarViewShown" property is what you need-->

{% endhighlight %}


Now, let's take another look.

{:.screenshot}
![](http://i.imgur.com/3F676Zj.png)

Better?

## Picker for Numbers/Text

Pickers are needed for more than just date/time. Pickers can serve as an alternative to drop-down lists for users to pick an item from a pre-defined list (for example: gender, age range, quantity). We can make use of the [`NumberPicker`](http://developer.android.com/reference/android/widget/NumberPicker.html) class to achieve this.

(For consistency, we will continue to put pickers in dialogs, but you can place it anywhere in your layout.)

The following examples will run through some short code snippets, a brief description of each snippet, and the resulting screenshot.

# Numbers

To create the most basic number picker, all we have to do is set the minimum and maximum values for the picker, and it will do the rest for us. (you can also set your own range, like numbers 11 through 20)

{% highlight java %}
NumberPicker myNumberPicker = new NumberPicker(this);
myNumberPicker.setMaxValue(10);
myNumberPicker.setMinValue(0);

NumberPicker.OnValueChangeListener myValChangedListener = new NumberPicker.OnValueChangeListener() {
  @Override
  public void onValueChange(NumberPicker picker, int oldVal, int newVal) {
    txtPickerOutput.setText("Value: " + newVal);
  }
};

myNumberPicker.setOnValueChangedListener(myValChangedListener);

new AlertDialog.Builder(this).setView(myNumberPicker).show();
{% endhighlight %}

{:.screenshot}
![](http://i.imgur.com/plFTwzb.png)

# Text

For this example, as well as the upcoming one, we will use an array of items to display them in your picker. We still have to set the min/max values for your picker; and in this case, they will be 0 and "length - 1" respectively. The difference is, that in this case, we pass our array to the picker by calling its `setDisplayedValues()` method.

{% highlight java %}
final String genders[] = { "Male", "Female" };

NumberPicker myNumberPicker = new NumberPicker(this);
myNumberPicker.setMinValue(0);
myNumberPicker.setMaxValue(genders.length - 1);
myNumberPicker.setDisplayedValues(genders);

NumberPicker.OnValueChangeListener myValChangedListener = new NumberPicker.OnValueChangeListener() {
  @Override
  public void onValueChange(NumberPicker picker, int oldVal, int newVal) {
    txtPickerOutput.setText("Value: " + genders[newVal]);
  }
};

myNumberPicker.setOnValueChangedListener(myValueChangedListener);

new AlertDialog.Builder(this).setView(myNumberPicker).show();
{% endhighlight %}

{:.screenshot}
![](http://i.imgur.com/n9bJhhN.png)

# Interval-based Numbers

As you will be able to guess from the associated snippet, this kind of a picker is very similar to the string picker, where we basically store the intended numbers as an array of strings, and display them in the picker.

{% highlight java %}
final ArrayList<String> numbersAsStrings = new ArrayList<>();

for (int i = 0; i <= 10; i++) {
  numbersAsStrings.add(String.valueOf(i * 10));
}

NumberPicker myNumberPicker = new NumberPicker(this);
myNumberPicker.setMinValue(0);
myNumberPicker.setMaxValue(numbersAsStrings.size() - 1);
myNumberPicker.setDisplayedValues(numbersAsStrings.toArray(new String[numbersAsStrings.size()]));

NumberPicker.OnValueChangeListener myValChangedListener = new NumberPicker.OnValueChangeListener() {
  @Override
  public void onValueChange(NumberPicker picker, int oldVal, int newVal) {
    txtPickerOutput.setText("Value: " + numbersAsStrings.get(newVal));
  }
};

myNumberPicker.setOnValueChangedListener(myValChangedListener);

new AlertDialog.Builder(this).setView(myNumberPicker).show();
{% endhighlight %}

{:.screenshot}
![](http://i.imgur.com/jTblnUr.png)

Please feel free to tweak the appearances of the pickers to your liking either programmatically, or via XML in your layout file.

## Thank You

In future posts, we will look further into pickers by looking at some libraries that implement advanced pickers, and/or custom pickers.

Thank you very much for reading!
