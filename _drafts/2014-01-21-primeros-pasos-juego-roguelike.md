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

Por tanto, ahora tenemos que crear todas las funciones que hemos llamado, concretamente son estas: 

- `initMap()`
- `initCell()`
- `initActors()`
- `drawMap()`
- `drawActors()`


<!-- [id_ref]: URL--> 
[url-source]: http://gamedevelopment.tutsplus.com/tutorials/how-to-make-your-first-roguelike--gamedev-13677
