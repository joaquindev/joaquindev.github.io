---
layout: post
title:  "First steps with Titanium"
date:   2013-12-29 12:39
categories: titanium appcelerator ios webview
---

First of all I recommend you to go check and start [Dawson Toth][dawson-toth] awesome Titanium tutorial. So, source:[Learning Titanium (Created by Dawson Toth)][source-inspiration]. 

The first thing to know in Titanium is that all applications start creating a window so we are going to create a window and store a reference to it in the variable `win`:

```javascript
var win = Ti.UI.createWindow();
```

Windows take a single argument: a *dictionary* which is another variable that will be send as an argument in the `createWindows()` method: 

```javascript
var dictionary = {};
```

As we only use our dictionary variable once, we can mix both ideas in just one: 

```javascript
var win = Ti.UI.createWindow({});
```

The dictionary variable can contain any [windows property][windows-properties-doc], for instance the property `backgroundColor`:

```javascript
var win = Ti.UI.createWindow({ backgroundColor: 'green' });
```
Other properties that goes in the window object can be: 

```javascript
var win = Ti.UI.createWindow({
  backgroundColor: 'white', 
  exitOnClose: true, 
  fullscreen: false, 
  title: 'title text' });
```

Next, we are already ready to make our window visible, to do so we can call the `open` method: 

```javascript
win.open();
```

###Views

The power of an application in Titanium is, amongst other important things, is the option of creating views on top of a window. In fact, [views][titanium-view-doc] are like an empty drawing canvas or a container. It is the base of all visible elements in Titanium. So let's define a `view` and add it to our window: 

```javascript
var view = Ti.UI.createView({
  backgroundColor: 'green',
  width: 100, height: 100
});
win.add(view);
```

As you think, views can be added inside other views like this: 

```javascript
var outerView = Ti.UI.createView({
  backgroundColor: 'red', 
  width: 100, height: 100
});

var innerView = Ti.UI.createView({
  backgroundColor: 'white', 
  width: 20, heigth: 20
});
outerView.add(innerView);
win.add(outerView);
```

Views can be positioned inside windows (or other views) in different ways: 

I. Placing it using distances: 

```javascript
win.add(Ti.UI.createView({ 
  backgroundColor: 'green', width: 20, height: 20, top: 10, left, 10
}));
```
II. Placing it using percentages: 

```javascript
win.add(Ti.UI.createView({
  backgroundColor: 'red', width: 20, height: 20, right: '20%', top: 20
}));
```
III. By explicitly defining the unit:

```javascript
win.add(Ti.UI.createView({
  backgroundColor: 'blue', width: 20, height: 20, bottom: '1in'
}));
```
###Labels

A [label][titanium-label-doc] is a type of view that lets you display text. Similar to views, we can create a label using `Ti.UI.createLabel()` method: 

```javascript
var win = Ti.UI.createWindow({ backgroundColor: '#fff' });

win.add(Ti.UI.createLabel({
  text: 'Text inside the label', textAlign: 'center', color: '#000'
});
```

###Buttons

A [button][titanium-button-doc] is just a type of view which can be clicked, so we use the `Ti.UI.createButton()` method and then we add the button to our window: 

```javascript
var button = Ti.UI.createButton({ title: "Click me, I'm a button!", textAlign: 'center', color: '#000'; }); 
```

Notice that we can change the alignment and color of our text button. Also, we can set *EventListeners* to our buttons to execute some Javascript code inside the web view: 

```javascript
button.addEventListener('click', function(event){
  alert('This is the alert JS code executed when you click this button');
});
```

###Graphics 

Graphics can be displayed in different ways: 

I. Using Image Views: 

The `ImageView` object is used to display a single image or series of animated images. 

```javascript
win.add(Ti.UI.createImageView({
  image: 'images/buttonImage.png', 
  width: 100, height: 50, top: 10, left: 10
}));
```

II. Using the [backgroundImage][titanium-backtroundimage-doc] property of views: 

```javascript
win.add(Ti.UI.createView({
  backgroundImage: 'images/button.png', 
  width: 100, height: 50, top: 90, left:10
}));
```

##Webviews

Let's build a simple web application based on a `webview`. First go to *File, New, Mobile Project, Classic, Single Windows Application*. Then, in *Project name* introduced **ExampleSingleWebApplication** and in *App id* type **com.examplesinglewebapplication.com**. Titanium SDK Version should be **3.1.3.GA** (at the time of writing this). And selected *Deployment Targets* checkboxes should be **iPad, iPhone, Mobile Web**. Click finish and close the first window that you see called *TiApp Editor* (you can load this info by clicking in `tiapp.xml` file later). The files in the project should be these: 

```bash
ExampleSingleWebApplication/
├── CHANGELOG.txt
├── LICENSE
├── LICENSE.txt
├── README
├── Resources
│   ├── KS_nav_ui.png
│   ├── KS_nav_views.png
│   ├── app.js
│   ├── iphone
│   │   ├── Default-568h@2x.png
│   │   ├── Default-Landscape.png
│   │   ├── Default-Portrait.png
│   │   ├── Default.png
│   │   ├── Default@2x.png
│   │   └── appicon.png
│   ├── mobileweb
│   │   ├── appicon.png
│   │   ├── apple_startup_images
│   │   │   ├── Default-Landscape.jpg
│   │   │   ├── Default-Landscape.png
│   │   │   ├── Default-Portrait.jpg
│   │   │   ├── Default-Portrait.png
│   │   │   ├── Default.jpg
│   │   │   ├── Default.png
│   │   │   └── README
│   │   └── splash
│   │       ├── README
│   │       ├── appc.png
│   │       ├── splash.css
│   │       ├── splash.html
│   │       └── titanium.png
│   └── ui
│       ├── common
│       │   └── FirstView.js
│       ├── handheld
│       │   ├── ApplicationWindow.js
│       │   └── android
│       │       └── ApplicationWindow.js
│       └── tablet
│           └── ApplicationWindow.js
├── i18n
│   ├── en
│   │   └── strings.xml
│   └── ja
│       └── strings.xml
├── manifest
└── tiapp.xml
```

As you can see, there is a `i18n` folder in charge of location languages and then we have our `Resources` folder containing our application (with 3 specific folders regarding the iPhone, Android and Mobile versions). Let's open `app.js`, it is the starting point where we take care of: 

1. Bootstrap the application with any data we need
2. Check for dependencies like device type, platform version or network connection
3. Require and open our top-level UI components.

###Loading Google in a webview

So we can go to `app.js` and erase all its content and just write this code to get google loaded into our webview: 

```javascript
var win = Ti.UI.createWindow({
    backgroundColor: 'white'
});

var web = Ti.UI.createWebView({
    url: 'http://www.google.com'
});

win.add(web);

win.open();
```

As you can see, the code is easy, we define our window then define our webview and finally add the webview to our window. 

###Loading our own HTML page in a webview

Right click in your project folder, select *New File* and name it **index.html**, then we type our very basics into our index.html: 

```html
<TYPE HTML>
<html>
<body>
<h1>This is my own HTML page</h1>
</body>
</html>
``` 

And then the only thing you have to change is `www.google.com` and type in our `index.html: 

```javascript
var web = Ti.UI.createWebView({
    url: 'index.html'
});
```
###Understanding more about our `app.js` template code

When we first create our project, there is already code in our `app.js`, let's understand what it does. First of all, we bootstrap and check dependencies, to do so we check `Ti.version` property to know our version: 

```javascript
if (Ti.version < 1.8 ) {
  alert('Sorry - this application template requires Titanium Mobile SDK 1.8 or later');   
}
```

Next we have to understand that as we've created a *Single Window Application*, we are going to stack our windows in a single context application, that is why we create a javascript anonymous function with the usual wrapper: 

```javascript
(function(){
...
})();
```
Then we render the appropriate components based on the platform and form factor: 

```javascript
    var osname = Ti.Platform.osname,
        version = Ti.Platform.version,
        height = Ti.Platform.displayCaps.platformHeight,
        width = Ti.Platform.displayCaps.platformWidth;
```

Next we have to take into account if the user agent comes from a tablet. Considering a tablet to have one dimension over 900px, this is imperfect, so you should feel free to decide yourself what you consider a tablet form factor for android: 

```javascript
var isTablet = osname === 'ipad' || (osname === 'android' && (width > 899 || height > 899));
```

Now it's time to define our window as we already know and since we've defined what a tablet view should be we can also check if we are loading our page in what we have decided to be a tablet: 

```javascript
var Window;
if (isTablet){
    Window = require('ui/tablet/ApplicationWindow');
}
else {
...
}
```

Finally, if it's not a tablet, it is a mobile device which can be an android or an ios device, so that is what we are going to check in the `else` section. However, don't forget that Android uses platform-specific properties to cretae windows and all other platforms follow a similar UI pattern:  

```javascript
if (osname === 'android') {
    Window = require('ui/handheld/android/ApplicationWindow');
}
else {
    Window = require('ui/handheld/ApplicationWindow');
}
new Window().open();
```


#**[Post in process]**
----------------------

[dawson-toth]: https://twitter.com/dawsontoth
[windows-properties-doc]: http://docs.appcelerator.com/titanium/2.1/#!/api/Titanium.UI.Window
[titanium-view-doc]: http://docs.appcelerator.com/titanium/2.1/index.html#!/api/Titanium.UI.View
[titanium-label-doc]: http://docs.appcelerator.com/titanium/2.1/index.html#!/api/Titanium.UI.Label
[titanium-button-doc]: http://docs.appcelerator.com/titanium/2.1/#!/api/Titanium.UI.Button
[titanium-backtroundimage-doc]: http://docs.appcelerator.com/titanium/2.1/#!/api/Titanium.UI.View-property-backgroundImage

[source-inspiration]: http://appc.me/learn/ 
