---
layout: post
title:  "Implementando leaderboard en Redis"
date:   2014-01-16 20:26
categories: leaderboard redis tabla puntuaciones 
---

Vamos a probar este módulo de NPM llamado [scoreboard][scoreboard-url]. Por tanto, accede su repositorio en el enlace anterior para instalar el módulo (que como siempre será con el comando `npm install scoreboard`).

###Levantando un servidor de Redis

Además vamos a utilizar un servidor local de Redis, para ello, seguiremos las instrucciones que nos explican en la web de [jasdeep.ca][installing-redis-on-mac]. Básicamente tendremos que desistalar el archivo comprimido y ejecutar el comando `make test` y a continuación el comando `make`. A continuación podemos por ejemplo entrar directamente a la carpeta `/src` para levantar el servidor en local ejecutando el comando `.redis-server`.

###Fichero app.js con el objeto Leaderboard

Tal como indica el repositorio del módulo NPM, declaramos el objeto leaderboard así: 

{% gist 8461961 %}

En el repositorio también nos explican cómo configurar la conexión con el servidor Redis pero como nosotros utilizamos el servidor local, la aplicación ya conecta directamente. 
###Insertando puntuaciones en la tabla 
 
Para insertar un dato a un usuario, simplemente tendremos que *indexar* el objeto que por ejemplo *hemos matado*, los *puntos* que nos han dado y finalmente el **usuario** al que le tenemos que sumar la puntuación. Repetiremos el ejemplo que viene en el repositorio es este: 

{% gist 8462067 %}

###Recuperando los datos de puntuación

Para recuperar la puntuación para por ejemplo saber quién va primero, utilizaremos el método leader() del objeto `scores` y encadenaremos la llamada al método `run` pasándole como argumentos los objetos `err` y `leaderboard`. Seguimos de nuevo el ejemplo del repositorio: 

{% gist 8462183 %}



<!-- [id_ref]: URL--> 
[scoreboard-url]: https://github.com/eschan/scoreboard
[installing-redis-on-mac]: http://jasdeep.ca/2012/05/installing-redis-on-mac-os-x/
