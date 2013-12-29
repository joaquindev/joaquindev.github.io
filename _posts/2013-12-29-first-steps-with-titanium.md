---
layout: post
title:  "First steps with Titanium"
date:   2013-12-29 12:39
categories: titanium appcelerator ios 
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




#**[Post in process]**
----------------------

[dawson-toth]: https://twitter.com/dawsontoth
[windows-properties-doc]: http://docs.appcelerator.com/titanium/2.1/#!/api/Titanium.UI.Window
[titanium-view-doc]: http://docs.appcelerator.com/titanium/2.1/index.html#!/api/Titanium.UI.View
[titanium-label-doc]: http://docs.appcelerator.com/titanium/2.1/index.html#!/api/Titanium.UI.Label
[titanium-button-doc]: http://docs.appcelerator.com/titanium/2.1/#!/api/Titanium.UI.Button
[titanium-backtroundimage-doc]: http://docs.appcelerator.com/titanium/2.1/#!/api/Titanium.UI.View-property-backgroundImage

[source-inspiration]: http://appc.me/learn/ 
