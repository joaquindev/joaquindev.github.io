---
layout: post
title:  "Primeros pasos con Pixi.js"
date:   2014-01-11 22:26
categories: videogames libraries 
---

Estas primeras impresiones sobre **Pixi.js** las he ido sacando conforme seguía el tutorial de [Peepsquest][peepsquest], os recomiendo que os paséis por su página y que estéis al tanto de su [repositorio de Github][repositoriopeepsquest].

##Inicializando los objetos Stage y Renderer

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

##Incluir una imagen en Pixi.js

Vamos a crear una imagen o un *sprite* que es lo que se considera una unidad visual básica en los videojuegos. La apariencia visual de un *sprite* es la textura que se le ha asignado: 

```javascript
var texture = PIXI.Texture.fromImage('img/soccerball8bitpixelatedfilter12.png');
var leaf = new PIXI.Sprite(texture);

//rotamos la hoja 
leaf.anchor.x = 0.5;
leaf.anchor.y = 0.5;

//la centramos en el stage
leaf.position.x = WIDTH / 2;
leaf.position.y = HEIGHT / 2;

//la situamos en el stage para que sea renderizada
stage.addChild(leaf);
```

##Realizar animación en Pixi.js

La forma de animar la imagen que hemos incluído dentro del *stage* va a ser solicitando un frame repetidamente (equivalente a como si pintásemos la misma imagen repetidamente) utilizando la misma función todo el tiempo, es decir recursivamente. Sin embargo la variación que vamos a realizar entre cada llamada y la siguiente es que vamos a realizar una ligera variación en la rotación: 

```javascript
function gameLoop(){
  requestAnimFrame(gameLoop);
  leaf.rotation -=0.02;
  renderer.render(stage);
}
requestAnimFrame(gameLoop);
```

#Descargando y ejecutando el código

Para entender y ver el resultado de la ejecución de estos primeros pasos, accede al [repositorio que he creado][repositoriotutorialpixijs] y descárgate el ejemplo. Una vez te lo hayas descargado, accede a la carpeta que contenga el archivo `index.html` y tenemos que ponerlo disponible a través de un servidor web básico, para ello podemos utilizar el que python nos ofrece con el comando:

```bash
python -m SimpleHTTPServer
```

Ya solo falta acceder a la página: `http://localhost:8000` para ver nuestro *hola mundo* con la biblioteca de desarrollo de videojuegos en lenguaje Javascript PIXI. 


<!-- [id_ref]: URL--> 
[peepsquest]: http://peepsquest.com/tutorials/pixi-basics.html
[repositoriopeepsquest]: https://github.com/peepsquest
[repositoriotutorialpixijs]: https://github.com/joaquindev/tutorial-pixijs
