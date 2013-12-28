---
layout: post
title:  "Conociendo por fin Awk"
date:   2013-12-26 11:45
categories: awk funcionamiento
---

A lo largo de la carrera suele ocurrir que tienes un comando en Linux que siempre ves de pasada pero que nunca te sientas a entender lo que realmente haces. Eso en el mundo de la informática ocurre mucho ya sí que somos conscientes de la gran potencia que tiene el Terminal en UNIX.

La finalidad de la herramienta **awk** es ayudarnos en los casos en los que tenemos que manipular datos aplicando tareas como cambiar el formato de un archivo, encontrar elementos con cierta característica y por ejemplo es utilizada en los casos en los que compilamos algunos paquetes desde sus fuentes. No es tiene un sistema de tipos de datos complicado ya que únicamente utiliza números y cadenas de caracteres.

Vamos a ver un ejemplo (archivoawk.dat), pongamos el caso que tenmos un fichero de datos en el que la primera columna son equipos de fútbol, la segunda es la puntuación que llevan en la liga y la tercera son los goles recibidos:

F. C. Barcelona 24    8   
Real Madrid     24    6
Sevilla F.C.    6     3
Equipo3         9     0
Valencia F. C.  16    6

Si por ejemplo queremos imprimir únicamente el nombre del equipo y el valor de los goles recibidos multiplicados por el valor de su puntuación, utilizando el lenguaje AWK lo haríamos así: 

{% highlight ruby %}
awk '$3 > 0 {print $1, $2*$3}'
{% endhighlight %}

Por último, con el comando *awk* también podemos ejecutar ficheros que contengan más código, para ello podremos lanzar un programa escrito en el lenguaje de programación *awk* utilizando la siguiente instrucción:

{% highlight ruby %}
awk -f programa.awk datos.awk
{% endhighlight %}

Y en comando más avanzado sería por ejemplo el eliminar duplicados del fichero *iptables* (Fuente: <https://twitter.com/climagic>): 
{% highlight ruby %}
awk '!a[$0]' /etc/sysconfig/iptables
{% endhighlight %}


Por último en cuanto a los parámetros de Awk, con **$0** haremos referencia a una línea entera mientras que con **$1** estaremos haciendo referencia al primer argumento de los pasados en una lista separados por comas.  

Fuentes: http://aprendiendoausarlinux.wordpress.com/2012/03/09/un-tutorial-de-awk/
