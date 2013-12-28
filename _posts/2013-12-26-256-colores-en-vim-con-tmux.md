---
layout: post
title:  "256 colores en VIM con Tmux"
date:   2013-12-26 11:28
categories: vim tmux colores
---

Lo primero que te das cuenta al comenzar a trabajar con Tmux es su gran flexibilidad y potencia. Sin embargo, al ser una herramienta cuyo entorno de trabajo es el Terminal puede que no todo funcione a la primera. De hecho, en mi caso, al estar dentro de Tmux y abrir un fichero con VIM, únicamente me cargaba un perfil de 16 colores, que aunque tiene su puntito retro, no me resultaba útil porque los comentarios en gris no los podía ver. Para solucionarlo, basta con incluir el siguiente comando en tu fichero .tmux.conf:

{% highlight ruby %}
set -g terminal-overrides 'xterm:colors=256'
{% endhighlight %}

Puede ser que utilices otro terminal por defecto, si ese es tu caso, tienes que cambiar *xterm* por el terminal que utilices. 

Fuente:[bentomas.com][fuente-256-colors-in-tmux] 

[fuente-256-colors-in-tmux]: http://bentomas.com/2012-03-28/256-colors-in-tmux.html
