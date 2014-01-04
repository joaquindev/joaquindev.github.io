---
layout: post
title:  "Basic videogame with canvas and requirejs"
date:   2014-01-04 11:14
categories: videogame canvas requirejs 
---

Sources: 
Based in the work of [N0NamedGuy][N0NamedGuy], watch his awesome work in his github repo [The_Thief][thethiefurl]. 

Lets work on our HTML5 videogame structure. We are going to use Javascript and HTML code to implement it. 

##index.html

This is going to be the starting point of our videogame and because it is a HTML game it will be played in a browser so it follows the standard `index.html` entry point. 

In here we are going to use the `viewport` metatag with the following properties/attributes: 

```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0" />
```

Next, we define the general style with: 

```css
  <style>
    canvas {
      image-rendering: crisp-edges;
      image-rendering: optimize-speed;
      image-rendering: optimize-contrast;
      image-rendering: -moz-crisp-edges;
      image-rendering: -webkit-optimize-contrast;
      -ms-interpolation-mode: nearest-neighbor;

      -webkit-touch-callout: none;
      user-select: none;
      -ms-user-select: none; 
      -webkit-user-select: none; 
      -khtml-user-select: none;
      -moz-user-select: none;
      -webkit-touch-callout: none;
      -webkit-user-drag: none;
      outline: none;
      -webkit-tap-highlight-color: rgba(255, 255, 255, 0); /*mobile webkit*/

      display: block;
      position: absolute;
      left: 0; 
      top: 0; 
      background-color: lightgray;
    }
    div#debug {
      background-color: lightgray;
      z-index: 999999;
    }
    html, body {
      background-color: black;
      width: 100%;
      height: 100%;
    }
    }
  </style>
```

And then we define an entry point so RequireJS can take control of the script management, we do so using `data-main` attribute that is going to be `game.js` (remember with require.js there is no need of using .js extension) and then we define where our RequireJS library is: 

```javascript 
<script type="text/javascript" data-main="js/game" src="js/lib/require.js"></script>
``` 
 
#POST IN PROGRESS
















[N0NamedGuy]: https://github.com/N0NamedGuy
[thethiefurl]:https://github.com/N0NamedGuy/The_Thief
