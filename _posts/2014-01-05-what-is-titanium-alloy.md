---
layout: post
title:  "What is Titanium Alloy"
date:   2014-01-05 09:57
categories: alloy titanium appcelerator 
---

If you follow Appcelerator Documentation and its [QuickStart example][appcelerator-quickstart] you may wonder what is the difference between starting a project based on an Alloy template or a Classic template. To start an Alloy project go to *New Mobile Project > Alloy > Default Alloy Project*.

##Alloy framework

Alloy is an MVC framework for Titanium which is developed by Appcelerator. Is a side project and you can find its source code in the [alloy github repository][alloy-github]. Just in case you don't know about the [MVC pattern][mvcpattern], we have three elements: views, controllers and models. But before, just let's have a look at what folders were created using the Alloy template: 

```bash
├── CHANGELOG.txt
├── LICENSE
├── LICENSE.txt
├── README
├── Resources
│   ├── alloy
│   ├── alloy.js
│   ├── android
│   ├── app.js
│   ├── blackberry
│   ├── iphone
│   ├── mobileweb
│   └── tizen
├── app
│   ├── README
│   ├── alloy.js
│   ├── assets
│   ├── config.json
│   ├── \controllers
│   ├── \models
│   ├── styles
│   └── \views
├── build
│   ├── alloy
│   ├── iphone
│   └── map
├── manifest
├── plugins
│   └── ti.alloy
└── tiapp.xml
```

You can see the structure of the MVC pattern under `app` folder, in fact we have our three main folders: `views`, `models` and `controllers`. You can also find a folder for storing your `styles` which we are also going to check.  

###View

The view declares the structure of the GUI. Views are going to be stored in the path `app/views/`, for example let's define a window with an image and label: 

```html
<Alloy> 
  <Window>
    <ImageView id="imageView" onClick="clickImage" />
    <Label id="l">Click Image</Label>
  </Window>
</Alloy>
```

###Style and assets

The style file formats and the style of the components are stored in `styles` folder. Let's define some style which follows a similar structure like CSS, in fact it has an extension very similar to CSS, the extension is `.tss`: 

```css
"Window": {
    backgroundColor:"white"
},
"#l":{
    bottom:20,
    width: Ti.UI.SIZE,
    height: Ti.UI.SIZE,
    color:'#999'
},
"#imageView":{
    image:"/images/apple_logo.jpg",
    width:24,
    height:24,
    top:100
}
```

If you take a look at the tree structrue of our application, there is also a folder called `assets`. This is the folder where we are going to store static files like images and icons. 

###Controllers 

The controller contains the presentation logic in charge of responding to user input. Controllers are coded in Javascript. Let's make an example that creates an alert dialog when the user clicks on the image and opens the window when the application starts: 

```javascript
function clickImage(e){
  Titanium.UI.createAlertDialog({title:'Image View Title', message:'Clicked!'}).show();
}
$.index.open();
```

If you take a look at the tree structrue of our application, there is also a folder called `assets`. This is the folder where we are going to store static files like images and icons. 

###More examples

If you are interested in following your application development with Alloy, check out [more examples][more-examples] available in the Appcelerator website. There are some useful examples that you should read: 

* [Geolocation][geolocation-example]
* [RSS Reader][rssreader-example]
* [Todo List][todolist-example]

###Wrapping up

So as we've seen, Alloy allows you to build your application in a more structured way. You have controll of everything like a controller, a view or a model (as well as assets and styles). However it is important that you remember that you can also build your application with titanium using just your usual web tools and then include your web and your stuff in the correct place. So whether you decide to build up your application using Alloy or as a standard web page, take a look at Titanium Studio to help you out. 



<!-- [id_ref]: URL--> 
[appcelerator-quickstart]: http://docs.appcelerator.com/titanium/latest/#!/guide/Quick_Start-section-37538717_QuickStart-HelloTitaniumApp
[alloy-github]:https://github.com/appcelerator/alloy
[mvcpattern]:http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
[more-examples]: http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_Samples
[geolocation-example]: http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_Samples-section-37535160_AlloySamples-Geolocation
[rssreader-example]: http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_Samples-section-37535160_AlloySamples-RSSReader
[todolist-example]: http://docs.appcelerator.com/titanium/latest/#!/guide/Alloy_Samples-section-37535160_AlloySamples-TodoList 
