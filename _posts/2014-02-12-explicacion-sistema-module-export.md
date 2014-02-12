---
layout: post
title:  "Cómo usar Module.exports"
date:   2014-02-12 07:36
categories: node.js module.exports 
---

Fuente/Source: [Johan's Thoughts][url-source]

Vamos a ver el uso del objeto ```module.exports``` junto con el uso de ```require``` en cualquier script de node. La idea principal es que en un módulo podemos poner lo que queramos que luego sea accesible desde fuera de esa zona de código de forma que al utilizar la instrucción ```require``` en tu código, estará disponible todo lo que has definido. 

####Uso

Seguramente, ya has estado haciendo uso de la función ```require```sin darte cuenta desde hace ya tiempo. En cualquier script de Node, usaremos ```require('nombre-del-modulo')``` para obtener el objeto que hayamos definido previamente, es por eso que siempre se suele utilizar asignándolo a una variable como por ejemplo ```var fs = require('fs')```. Por otro lado, está la definición del propio módulo, para ello haremos uso de la función ```module.exports``` en la que estaremos creando un objeto que guardará referencias. 

####Exportando funciones

Vamos a definir un módulo, por ejemplo este: 

{% gist 8951066 %}

Como puedes ver, se trata de un fichero que contiene tres funciones y una variable. Si te estas preguntando cómo podemos hacer uso de dichas funciones desde fuera del fichero y hacer uso de sus funciones como hacíamos ya en C, la respuesta es haciendo uso de ```module.exports```. Para ello, definimos dentro del objeto exports lo que queremos que esté accesible y con qué nombre estará disponible. En el caso de definir un nombre, podemos aprovechar para acortar el nombre de alguna función pero su uso ha de ser controlado porque sino estaremos más tiempo buscando la correspondencia entre los nombres de las funciones que consultando qué hacen dichas funciones. 

Ahora, desde cualquier otro script de Node podemos *traernos* dichas funciones para utilizarlas: 

{% gist 8951082 %} 

Puedes observar que estamos utilizando la misma nomenclatura que llevas usando con los paquetes de NPM desde hace tiempo, la idea es definirse una variable a través de la cual podremos acceder a todas las cosas que hayamos definido previamente en ```module.exports```. 

###La opción Orientada a Objetos

En el caso de que lo único que queramos es devolver una única función por módulo (de forma que sea el único punto de entrada), como por ejemplo una función constructora, podemos *exportar* directamente dicho elemento: 

{% gist 8951172 %}

Y luego desde cualquier lugar podemos hacer: 

{% gist 8951189 %}

###Exportando una biblioteca completa

Una vez que hemos declarado varios módulos que queremos exportar, es conveniente entonces que empieces a construir una biblioteca (o librería si sigues usando el término mal traducido del inglés library). Para ello, los pondremos todos en un mismo directorio y crea un fichero ```index.js``` que será el punto de entrada a los módulos, es decir ```src/miBiblioteca/index.js```. Lo que conseguimos con esto es que luego, ya podremos hacer uso de la biblioteca entera. Es decir, dentro de nuestro directorio de la biblioteca, tendremos un fichero index.js que tendrá una línea por fichero: 

{% gist 8951244 %}

Y luego ya, para hacer uso de todos los módulos utilizaremos el nombre del fichero en la instrucción require: 

{% gist 8951266 %} 

Espero que ya comprendas mejor el uso de ```module.exports``` ya que es una forma muy útil, y como siempre con Node.js, muy modular y potente. 

PS: Hay un alias llamado ```exports``` pero no se recomienda su uso porque es un poco lioso.

<!-- [id_ref]: URL--> 
[url-source]: http://coppieters.blogspot.com.es/2013/03/tutorial-explaining-module-export.html
