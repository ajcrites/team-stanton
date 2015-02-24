---
layout: post
title:  "WebViews in Android"
date:   2015-02-14 13:41:00
category: android
author: ashajgotra
---

Android delivers apps in three ways. First is the client-side (native) app developed using the Android SDK and delivered as an installable APK. The second delivery method is for a web application developed using web standards and made available to Android through a native Android web browser application. Finally, we can deliver a web application through an embedded web browser instantiated within a native Android APK using the [WebView](http://developer.android.com/reference/android/webkit/WebView.html) API. In this article, we will explore how to use the WebView API along with strengths and weaknesses compared to native display or browser display.

## WebViews in Android

WebView is the basic building block for displaying web pages in Android. It can be used to display any online content during an activity or could be used to build your own browser. It provides functionality like zooming, text searches, copy/paste and other built-in actions users expect in order to interact with web pages. By default, a WebView will not execute JavaScript, ignores web errors supporting, and only allows basic HTML functionality.

### WebView Uses

Making use of Webviews makes sense:

* when rendering information that will be updated regularly (e.g. user guide). In such case it is better to display an HTML document in a webview versus hardcoding into the application.
* if the application requires active network connection to fetch data (e.g. email). Rather than retrieving data via a network call and displaying in layout, it is better to design a responsive web page and render HTML in a WebView.

Avoid using Webviews:

* when the requirement is to have all the features of a web browser. WebView lack features such as navigation and the address bar. If you need these featuers, it makes sense to use default web browser application.

### Adding WebView to App

A WebView can be added to an Anroid application following these simple steps:

* Include `<WebView>` element in activity layout. e.g.
	
	{% highlight xml %}
	
	<?xml version="1.0" encoding="utf-8"?>
	<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
	    android:id="@+id/webview"
	    android:layout_width="fill_parent"
	    android:layout_height="fill_parent"
	/>
	
	{% endhighlight %}
* load web page in the WebView using `loadUrl()`.
	
	{% highlight java %}
	
	WebView myWebView = (WebView) findViewById(R.id.webview);
	myWebView.loadUrl("http://www.example.com");
	
	{% endhighlight %}
* app must have `INTERNET` permission in manifest file

	{% highlight xml %}
	
	<manifest ... >
	    <uses-permission android:name="android.permission.INTERNET" />
	    ...
	</manifest>
	
	{% endhighlight %}

#### Using JavaScript

JavaScript is disabled for a WebView by default. It can be enabled as shown below:

	{% highlight java %}
	
	WebView myWebView = (WebView) findViewById(R.id.webview);
	WebSettings webSettings = myWebView.getSettings();
	webSettings.setJavaScriptEnabled(true);
	
	{% endhighlight %}
	
	
#### Binding JavaScript to Android code

 `JavaScript` code in web pages can be bound to Android code using `interfaces`. e.g. we can define a class as below
 
 {% highlight java %}
 
 public class WebAppInterface {
     Context mContext;

     /** Instantiate the interface and set the context */
     WebAppInterface(Context c) {
         mContext = c;
     }

     /** Show a toast from the web page */
     @JavascriptInterface
     public void showToast(String toast) {
         Toast.makeText(mContext, toast, Toast.LENGTH_SHORT).show();
     }
  }

{% endhighlight %}
	
To make the desired method available to JavaScript from Android 4.2 `@JavascriptInterface` needs to be added to the method. Then bind the WebView with the interface as:

 {% highlight java %}
 
 WebView webView = (WebView) findViewById(R.id.webview);
 webView.addJavascriptInterface(new WebAppInterface(this), "Android");

{% endhighlight %}

Here the interface has been named `Android`. Then finally ths `showToast` method can be made available to JavaScript as:
 
 {% highlight html %}
 
 <input type="button" value="Say hello" onClick="showAndroidToast('Hello Android!')" />

 <script type="text/javascript">
     function showAndroidToast(toast) {
         Android.showToast(toast);
     }
 </script>

{% endhighlight %}

#### Handling Page Navigation
	
App users can be provided the functionality of opening a link in the WebView rather than opening it in browser although the best practise is to avoid letting user open web pages from links within the webview. 

In order to prevent such behavior, WebView needs to be connected to the webviewclient as:

{% highlight java %}

	WebView myWebView = (WebView) findViewById(R.id.webview);
	myWebView.setWebViewClient(new WebViewClient());
	
{% endhighlight %}

We can control the tool used to open the link by overriding the `shouldOverrideUrlLoading()` method of  `WebViewClient` in this way:

{% highlight java %}

	private class MyWebViewClient extends WebViewClient {
	    @Override
	    public boolean shouldOverrideUrlLoading(WebView view, String url) {
	        if (Uri.parse(url).getHost().equals("www.example.com")) {
	            // This is my web site, so do not override; let my WebView load the page
	            return false;
	        }
	        // Otherwise, the link is not for a page on my site, so launch another Activity that handles URLs
	        Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
	        startActivity(intent);
	        return true;
	    }
	}
	
	/// assigning customized WebViewClient to WebView
	WebView myWebView = (WebView) findViewById(R.id.webview);
	myWebView.setWebViewClient(new MyWebViewClient());
	
{% endhighlight %}

So now if the link points to `www.example.com` then it would be opened in the WebView otherwise browser app would be called up.

#### Navigating Web Page History

While overriding URL loading, WebView accumlates history of visited web pages which can be navigated using `goback()` and `goforward()` methods. To check whether there is any page for back or front navigation, we can use `canGoBack()` or `canGoForward()` methods as shown below:

{% highlight java %}

	@Override
	public boolean onKeyDown(int keyCode, KeyEvent event) {
	    // Check if the key event was the Back button and if there's history
	    if ((keyCode == KeyEvent.KEYCODE_BACK) && myWebView.canGoBack()) {
	        myWebView.goBack();
	        return true;
	    }
	    // If it wasn't the Back key or there's no web page history, bubble up to the default
	    // system behavior (probably exit the activity)
	    return super.onKeyDown(keyCode, event);
	}
	
{% endhighlight %}

### Loading HTML Files from File System

Webviews also provide advantage for apps supporting offline mode by providing the functionality to loads assets from local file system. `assets` directory is located in `src/main/assets` and can be used to store HTML, JavaScript, CSS files etc.. From this `assets` directory we can load an HTML file like this:

{% highlight java %}

mWebView.loadUrl("file:///android_asset/www/index.html");

{% endhighlight %}

#### Enabling Zoom feature

Default zoom functionality for a webview can be set using `WebSettings.setBuiltInZoomControls(boolean)` method.

# Customizing Webviews

We can customize a WebView to better meet our needs:

* Using `WebChromeClient` subclass we can control the UI of the browser (e.g. progress updates and JavaScript alerts).
* `WebViewClient` class controls the rendering of the content (e.g. form submission).
* `WebSettings` provides many setting options such as enabling JavaScript through `setJavaScriptEnabled()`
* As described earlier through `addJavascriptInterface(Object,String)` method Java objects can be injected into a WebView.

### Supporting Different Screen Densities

Android devices are divided into three categories based on screen densities: low, medium and high. By default a WebView scales the web page for a medium density device. You can target appropriate device densities to use certain features:

* First `window.devicePixelRatio` DOM property which specifies the scaling to be applied. If its value is `1.0` the targeted device is medium density one and default scaling is used, if value is `1.5` device is having high density and scaling of `1.5` is applied and value of `0.75` scales by the same amount for low density device.
* `-webkit-device-pixel-ratio` CSS media query which supports value of 0.75,1,1.5 respectievly for low, medium and high density devices. e.g. high density devices can be targeted as `<link rel="stylesheet" media="screen and (-webkit-device-pixel-ratio:1.5)" href="hdpi.css" />`
	
### Enhancements in KitKat

New functionalities have been added to WebView in KitKat version which makes it quite different from earlier versions. The new WebView is based on [Chromium](http://www.chromium.org/Home) which is an open-source project aimed at building faster, safer and more stable web browsers. It allows a WebView to support HTML5, CSS3 and JavaScript to match latest web browsers. 

### User Agent Changes

A browser's user agent string helps to identify which browser is being used, what version and on which operating system. In Kitkat `chrome` version has been added to it.

`Mozilla/5.0 (Linux; Android 4.4; Nexus 4 Build/KRT16H) AppleWebKit/537.36
(KHTML, like Gecko) Version/4.0 **Chrome/30.0.0.0** Mobile Safari/537.36`

Also if user agent string needs to be overridden `getUserAgentString()` method can be used otherwise static `getDefaultUserAgent()` method is good.

### Multi-threading and Thread Blocking

WebView should always excute on the main UI thread. To insure this `runOnUiThread()` method can be used as:

{% highlight java %}

	runOnUiThread(new Runnable() {
	    @Override
	    public void run() {
	        // Code for WebView goes here
	    }
	});
	
{% endhighlight %}

Also following Android standards the main UI thread should never be blocked. So waiting for a JavaScript callback on main thread is a bad practice. Instead use the `evaluateJavascript()` method to asynchronously evaluate JavaScript for the currently loading page.

### Custom URL Handling

The new WebView does not support custom URLs when requesting resources or resolving links. The URLs specified should confirm to [RFC 3986](http://tools.ietf.org/html/rfc3986#appendix-A).

### Viewport Changes

Earlier Android version used to support property called `target-densitydpi` which indicated the supported screen densities. This has been deprecated in Kitkat.

In Kitkat, the value specified for viewport width or height is adhered and WebView zooms in to fill screen width.

Kitkat only supports the last defined viewport tag unlike earlier version which used to combine properties from all tags.

`getDefaultZoom()` and `setDefaultZoom()` methods used to get and set the page zoom level have also been deprecated. 
 
### Styling Changes

`NARROW_COLUMNS` and `SINGLE_COLUMN` values for `WebSettings.LayoutAlgorithm` have been deprecated for more efficient `TEXT_AUTOSIZING` value.

### Handling Touch Events in JavaScript

In KitKat, handling `touchcancel` event is must for handling touch events in webview. It will be called when an element is touched and page is scrolled. 

### WebView Upgrades in Lollipop

* Added new security and stability enhancements
* User-agent string has been updated to version number `37.0.0.0`
* Now WebView can be granted access to resources like camera and microphone through `PermissionRequest` class
* Through `onShowFileChooser()` method Webview can be granted access to images and files
* Support for open standards like `WebAudio, WebGL, and WebRTC` have been introduced.
* If the target API is set to API level 21 then the system allows use of mixed content and third-party cookies. It allows carefully choosen HTML page portions to be displayed helping to reduce memory consumption and increase performance.
