---
layout: post
title:  "Construyendo un juego de mazmorras"
date:   2014-01-21 11:16
categories: phaser.js phaser mazmorras tutorial
---

Fuente: [How to make your first Roguelike by Ido Yehieli][url-source] 

Lo primero que hacemos es ser conscientes de las variables que vamos a necesitar para implementar nuestro juego, ya que cada juego va a ser diferente y cada mecánica de juego que quieras construir puede que necesite diferentes variables. Por ejemplo, dado que nuestro juego tiene un protagonista, necesitaremos una variable `player`. Pero además, como también queremos enemigos necesitaremos una lista de enemigos o actores, por tanto la llamaremos `actorList`. Y habrá que distinguir entre los enemigos muertos y vivos por lo que también necesitaremos una lista de los enemigos que todavía están vivos, para ello crearemos `livingEnemies`.

Además casi todos los juegos tienen una variable mapa, que llamaremos `map` y que contiene la estructura del mapa; una variable *pantalla* que llamaremos `screen` y que contiene un array de caracteres en formato ASCII con los elementos en la pantalla; y un mapa de nuestro actor llamado `actorMap` que va a contener los puntos hasta cada una de las posiciones de los actores, que será utilizada para realizar búsquedas rápidas. 

Ahora continuamos con algo fácil que tendremos que tener claro siempre en nuestros juegos, las **constantes**. En nuestro juego de mazmorras vamos a necesitar varias constantes: lo primero es definir el número de actores que queremos que haya por nivel, en nuestro caso definimos 10 actores por nivel en la constante `ACTORS` (aunque recordemos que en Javascript nada es constante, por ejemplo Chrome nos permite utilizar el modificador de variable `const` pero no es recomendable ya que en otros navegadores no se asegura su uso; lo único que vamos a hacer es definir todas las variables que representen a una constante en mayúsculas). Finalmente, definimos las dimensiones de nuestro mapa, para ello definimos: `ROWS` y `COLS` así como las dimensiones del texto que se va a mostrar en `FONT`. Por tanto, hasta ahora tenemos en nuestro `script.js` el siguiente código javascript: 

{% gist 8537649 %}

Ahora vamos con la creación del objeto **Phaser**, es decir el punto de entrada a nuestro videojuego. Para ello, utilizaremos el objeto `Game` de dentro de la biblioteca *Phaser*. Este objeto es el corazón de nuestro juego y nos dará acceso a las funciones que necesitamos para llevar a cabo nuestro juego. Los parámetros que tiene son: el ancho del juego en píxels `width`, el alto del juego en píxels `height`, el motor de renderizado a utilizar `Phaser.AUTO` al dejarlo en automático se escogerá el que funcione pero podríamos haber especificado: `Phaser.WEBGL`, `Phaser.CANVAS` o `Phaser.HEADLESS`. El siguiente parámetro va a ser el nodo del DOM que hará de padre de nuestro juego. Después viene la descripción del juego y para ello se utiliza un objeto JSON en el que le indicamos qué funciones tendrá que utilizar en el momento de `create` en el momento de `update`, etc. Por último, hay dos parámetros más: utilizar o no un fondo de pantalla transparente en el canvas y activar el **antialiasing** en los gráficos.

{% gist 8537940 %}

Tras declarar nuestro objeto `Game`, vamos ahora con la función que definimos en la descripción del juego, vamos a escribir la función `create()`. Empezamos inicializando la posibilidad de utilizar teclas, para ello ponemos un callback para que sea ejecutado cada vez que se produce el evento `onKeyUp` así `addCallbacks(null, null, onKeyUp)`. A continuación vienen los procesos de inicialización. Primero inicializamos el mapa con la función `initMap()`, después la pantalla con un bucle que recorrerá todas las celdas y las rellenará con el resultado de la función `initCell()`. Por último, inicializamos los actores con `initActors()` y ya finalmente dibujamos la pantalla y los actores con `drawMap()` y `drawActors(). El código resultante será así: 

{% gist 8538074 %} 

Por tanto, ahora tenemos que crear todas las funciones que hemos llamado, concretamente son estas, que como ves corresponden básicamente llevar a cabo la inicialización de los objetos implicados: 

- `initMap()`
- `drawMap()`
- `initCell()`
- `drawActors()`
- `initActors()`

###initMap()

En esta función creará un nuevo mapa aleatorio, para ello, iremos recorriendo todas las casillas de la matriz que representa el mapa y rellenaremos con mayor probabilidad con el carácter que representa una pared que un espacio libre, para ello haremos uso de la variable `map`:

{% gist 8558198 %} 

###drawMap()

Esta función es la encargada de pasar el contenido de la variable `map` a la matriz que representa la pantalla llamada `screen`, por tanto simplemente recorreremos el mapa y realizaremos la asignación a la propiedad `content` de la variable `screen`: 

{% gist 8558244 %}

###initCell()

Esta función se utiliza dentro de la función `create` y es la encargada de devolver un valor que será el que *pushearemos* al objeto `newRow` que será cada nueva línea del objeto `screen`. Recordemos que estas celdas van a contener caracteres ascii por lo que al insertarla se van a poner con unas características determinadas: 

{% gist 8558380 %}

###drawActors()

Con esta función dibujaremos en cada una de las posiciones de cada uno de los actores (definida por las coordenadas `actorList[a].x` y `actorList[a].y`), el número de puntos de vida que le quedan a dicho actor. Para ello justo en el momento de dibujar cada actor, también comprobaremos con una condición *inline* los puntos de vida que le quedan o una letra **e** en caso de que el personaje esté intacto:

{% gist 8558525 %}












##Funciones auxiliares

###randomInt(max)

Se trata de una función que utilizaremos para obtener un valor aleatorio el cual será como máximo el valor `max`: 

{% gist 8558282 %}

###canGo(actor, dir)

//TODO

###moveTo(actor, dir)

//TODO

###onKeyUp(event)

//TODO

###aiAct(actor)

//TODO


<!-- [id_ref]: URL--> 
[url-source]: http://gamedevelopment.tutsplus.com/tutorials/how-to-make-your-first-roguelike--gamedev-13677
