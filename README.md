# Using-Google-Analytics-with-Meteor

There are Mainly two ways to include Google Analytics in Meteor app 
* [Analytics JS](https://developers.google.com/analytics/devguides/collection/analyticsjs/)
* [Meteor Packages](https://atmospherejs.com/)

## [Analytics JS](https://developers.google.com/analytics/devguides/collection/analyticsjs/)

#### Adding analytics.js to Your Site

The analytics.js library is a JavaScript library for measuring how users interact with your website. This document explains how to add analytics.js to your site.

```
<!-- Google Analytics -->
<script>
(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
})(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

ga('create', 'UA-XXXXX-Y', 'auto');
ga('send', 'pageview');
</script>
<!-- End Google Analytics -->
```

'UA-XXXXX-Y' is tracking id of your Google Analytics Account

#### Page Tracking

Page tracking allows you to measure the number of views you had for a particular page on your website. Pages often correspond to an an entire HTML document, but they can also represent dynamically loaded content; this is known as "virtual pageviews".

```
ga('send', 'pageview', location.pathname);
```

#### Event Tracking

Events are user interactions with content that can be tracked independently from a web page or a screen load. Downloads, mobile ad clicks, gadgets, Flash elements, AJAX embedded elements, and video plays are all examples of actions you might want to track as Events.

```
function handleOutboundLinkClicks(event) {
  ga('send', 'event', {
    eventCategory: 'Outbound Link',
    eventAction: 'click',
    eventLabel: event.target.href
  });
}
```

> For event tracking, you need to track page view otherwise event won't be logged on Google analytics.

## [Meteor Packages](https://atmospherejs.com/)

To add Google Analytics in Meteor app I will use __reywood/meteor-iron-router-ga__ meteor package 

### iron-router-ga

Google analytics (universal edition) for Meteor with some Iron Router sugar for tracking page views and doing A/B testing with Content Experiments.

#### Installation

```
$ meteor add reywood:iron-router-ga
```

#### Meteor Settings

Configure analytics by adding a ga section to the public section of your Meteor settings. The only required property is id which is your Google Analytics tracking ID.

```
{
    "public": {
        "ga": {
            "id": "UA-XXXX-Y"
        }
    }
}
```

Advanced configuration options:

For more info on possible values for the configuration options below, check out Google's Advanced Configuration page.

* `create` -- object literal

  Options you would like to pass to ga("create", "UA-XXXX-Y", ...). Should have any of cookieDomain, cookieName, cookieExpires, etc as properties.

* `set` -- object literal

  Settings to apply to the tracker with ga("set", ...). These include forceSSL, anonymizeIp, etc.

* `require` -- object literal

  Additional tracking options to require with ga("require", ...) such as Display Advertising Features or Enhanced Link Attribution. For features like displayfeatures that don't have a corresponding *.js parameter (as linkid does), simply set the property value to true.

* `trackUserId` -- boolean

  If set to true, the currently logged in user's ID will be sent with all pageviews, events, etc. Learn more about user ID tracking. Defaults to false.
  
  Advanced configuration example:

```json
{
    "public": {
        "ga": {
            "id": "UA-XXXX-Y",
            "create": {
                "cookieDomain": "example.com",
                "cookieName": "my_ga_cookie",
                "cookieExpires": 3600
            },
            "set": {
                "forceSSL": true,
                "anonymizeIp": true
            },
            "require": {
                "displayfeatures": true,
                "linkid": "linkid.js"
            },
            "trackUserId": true
        }
    }
}
```


## Usage

Once you have installed this package and added the required configuration, you will have access to the `ga` function anywhere in your client-side Meteor code. You can use it just as you would on any other site.

### Event Tracking

[Tracking events](https://developers.google.com/analytics/devguides/collection/analyticsjs/events) is the same as always:

```javascript
ga("send", "event", category, action, label, value);
```

### Page View Tracking

Tracking page views is accomplished by adding configuration options to Iron Router. You can enable page view tracking for every route site-wide by configuring the router like so:

```javascript
Router.configure({
    trackPageView: true
});
```

Alternately, you can enable tracking for certain routes individually like so:

```javascript
Router.route("routeName", {
    // ...
    trackPageView: true
    // ...
});

// ** or **

Router.map(function() {
    this.route("routeName", {
        // ...
        trackPageView: true
        // ...
    });
});
```

If you have enabled site-wide page view tracking but want to disable it for a certain route:

```javascript
Router.route("routeName", {
    // ...
    trackPageView: false
    // ...
});
```

## Cordova settings

If you are building a Cordova app, you will need to allow your app to access the Google Analytics servers. Add the following snippet to your [`mobile-config.js`](https://docs.meteor.com/#/full/mobileconfigjs) file.

```javascript
App.accessRule('*.google-analytics.com/*');
```

### Debugging of Google Analytics

To debug the logging of Google Analytics, you need to install [Tag Assistant - Google](https://chrome.google.com/webstore/detail/tag-assistant-by-google/kejbdjndbnbjgmefkgdddjlbokphdefk?hl=en) 

Tag Assistant helps to troubleshoot installation of various Google tags including Google Analytics, Google Tag Manager and more.

![Figure 1](https://github.com/virtualforce/Using-Google-Analytics-with-Meteor/blob/master/Google%20Analytics.png "Figure 1")

After enable recording if your Google Analytics are correctly configured then Tag Assistant shows you the data of Page Views ,Event Tracking and Traffic 

![Figure 2](https://github.com/virtualforce/Using-Google-Analytics-with-Meteor/blob/master/Google-Analytics-2.png "Figure 2")

> For more help about Tag Assistanct, please watch this Youtube video:

[Learn About The New & Improved Google Tag Assistant](https://www.youtube.com/watch?v=4AqanTBA9X4)
