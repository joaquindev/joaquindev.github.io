---
layout: post
title:  "Primeros pasos con Pixi.js"
date:   2014-01-11 22:26
categories: videogames libraries 
---

Estas primeras impresiones sobre **Pixi.js** las he ido sacando conforme seguía el tutorial de [Peepsquest][peepsquest], os recomiendo que os paséis por su página y que estéis al tanto de su [repositorio de Github][repositoriopeepsquest].

#Inicializando los objetos Stage y Renderer

El primero objeto, el objeto *Stage* va a ser lo primero que vamos a crear. Este objeto será la pantalla o el área rectangular donde pondremos nuestro contenido. El segundo objeto que vamos a conocer es el objeto *renderer* el cual es el encargado de dibujar el contenido en el *stage* bien sea usando el objeto **Canvas** o mediante **WebGL** dependiendo de las características que tenga el navegador. Además, definiremos también el ancho, el alto y el color de fondo del *stage*:

```javascript
var WIDTH = 400;
var HEIGHT = 300;
var stage = new PIXI.Stage(0xEEFFFF);

//PIXIJS decidira si usar WebGL o Canvas
var renderer = PIXI.autoDetectRenderer(WIDTH, HEIGHT);

//Conectamos el Renderer a la pagina
document.body.appendChild(renderer.view);
```






<!-- [id_ref]: URL--> 
[peepsquest]: http://peepsquest.com/tutorials/pixi-basics.html
[repositoriopeepsquest]: https://github.com/peepsquest
