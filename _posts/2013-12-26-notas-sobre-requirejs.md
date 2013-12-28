---
layout: post
title:  "Empezando con RequireJS"
date:   2013-12-26 11:45
categories: notas requirejs javascript
---

Tengo una aplicación en AngularJS que poco a poco va creciendo. Conforme va aumentando el número de controladores, bibliotecas externas, etc, todo apunta a que el espíritu del [*Spaghetti code*][spaghetti-code-wikipedia] puede hacer acto de presencia y no irse nunca más. Con la finalidad de evitar que el código esté poco estructurado y además para ayudar con la definición, velocidad y carga aíncrona de bibliotecas y sus dependencias ([AMD][amd-definition]) empecé a utilizar [**RequireJS**][requirejs-web], una librería que realiza labores de orquestación u organizador a la hora de cargar ficheros y módulos en Javascript el cual está optimizado para su uso en navegadores web (aunque también puede ser utilizado en otros entornos como Rhino o Node.js). 

[spaghetti-code-wikipedia]: http://en.wikipedia.org/wiki/Spaghetti_code
[amd-definition]: https://github.com/amdjs/amdjs-api/wiki/AMDamd-definition]
[requirejs-web]: http://requirejs.org/

Lo primero tras descargarnos RequireJS (a partir de ahora RJS) es entender que asume que todos los scripts estarán en el directorio */scripts* dentro de nuestro proyecto. Por ejemplo, si tenemos un proyecto que tiene una página *project.html*, la cual utiliza diferentes scripts, la estructura del directorio tendrá *project.html* en el raíz y luego un directorio *scripts*. Al incluir RJS en nuestro proyecto, éste también irá en el directorio de scripts.

Desde la web de RJS nos recomiendan que para sacarle el máximo partido evitemos tener secciones de código dentro de scripts en las páginas html y que únicamente referenciemos la carga de RJS en la sección *head*: 

{% highlight ruby %}
<script data-main="scripts/main" src="scripts/require.js"></script>
{% endhighlight %}

Es importante recordar que esta estructura es una estructura sugerida por la documentación de RJS pero que puede ser adaptada a tus necesidades. Por ejemplo la utilización de bibliotecas de terceros suele estar situada en un directorio llamado */vendor* por lo que puedes organizar la estructura como quieras, siempre y cuando respetes la localización de RJS y que sepa a dónde tiene que ir a buscar el fichero *data-main* que es el punto de entrada o primer paso que realiza RJS. Además, RJS automáticamente va a colocar la extensión .js a todos los ficheros por lo que no es necesario que estemos escribiendo extensiones de javascript (la razón es que RJS asume que todas las dependencias van a ser scripts).  

Ahora, vamos a comenzar con el contenido del fichero main (main.js). Este fichero es el que informa a RJS sobre las cosas que tiene que cargar y ejecutar además de controlar las opciones de configuración de RJS. Así estaremos asegurando un único punto de entrada en la aplicación (es por esto que se recomienda evitar declarar el uso de otras bibliotecas JS en el documento HTML sino que únicamente tendremos la línea de declaración de RJS). 

##Cargando Ficheros JavaScript

RJS tiene un enfoque diferente a la hora de cargar scripts comparado con la conocida etiqueta script. El objetivo principal de RJS es motivar la creación de código de forma modular y en esta línea, sugiere utilizar identificadores de módulos en lugar de URLs. RJS carga todo el código relativo a lo que lllama *baseUrl*, al utilizar el atributo *data-main* en la carga de RJS en el HTML, ese será la URL base a partir de entonces, es decir la misma ruta que tiene el fichero main.js, aunque la URL base puede ser configurada manualmente (consultar la documentación oficial para más detalle al respecto). Es decir, si recordamos la línea que incluímos en la página HTML: 

{% highlight ruby %}
<script data-main="scripts/main" src="scripts/require.js"></script>
{% endhighlight %}

Lo que va a hacer es define la URL base apuntando al directorio scripts y cargará un script cuyo identificador de módulo sea main. Por último, si no se especifica una URL base, RJS utilizará por defecto la ruta en la que se está ejecutando la página HTML que carga RJS. 

Ahora bien, si en algún momento no queremos hacer uso de esta funcionalidad de trabajar con una URL base e identificadores de módulos, de forma que queremos tratar el script como una URL relativa al documento en el que se esté usando, podemos usar tres mecanismos: El script termina en .js; el script empieza con */* y escribir literalmente el protocolo de transporte de datos como *http* o *https*. 

Aunque podemos estar tentados de seguir utilizando la nomenclatura tradicional en RJS, resulta interesante acostumbrarse al formato de localizar las cosas por el identificador del módulo ya que esto nos permitirá tener una flexibilidad mayor a la hroa de renombrar cosas en el futuro. 

##Separando el bibliotecas de terceros del código de nuestra aplicación

Para evitar tener montones de código de configuración todo junto, desde RJS se nos recomienda que evitemos tener jerarquías de directorios con mucha profundidad. En su lugar nos recuerdan que es mejor tener todos los scripts a mano a través de la URL base. No obstante, también nos puede interesar separar el código de nuestra aplicación y el código de las bibliotecas de terceros que incluyamos en nuestro proyecto. Para esto, se nos sugiere una estructura de directorios en la que separemos el directorio de nuestra aplicación y otro directorio que podemos llamar */lib* o */vendor* en el que incluyamos bibliotecas de terceros como por ejemplo *jquery.js* o *canvas.js*. Por tanto tendremos index.html y luego el punto de entrada por ejemplo en *js/app.js* y luego dentro de ese mismo directorio /js podremos tener otros dos directorios /app para nuestro código y /lib para las bibliotecas de terceros. El código que pondremos en main.js será este: 

{% highlight ruby %}

<script data-main="js/app" src="scripts/require"></script>

requirejs.config({
  baseUrl: 'js/lib',//Cargaremos todos los identificadores de módulos en js/lib
  paths: {
    app: '../app'
  }
});

requirejs(['jquery', 'canvas', 'app/sub'], function($, canvas, sub){
  //...
})
{% endhighlight %}

##Definiendo un módulo

En RJS un módulo no es lo mismo que un script tal y como lo conocemos tradicionalmente. La principal diferencia es que crea un objeto con su propio entorno (*scope*) de manera que evitaremos contaminar el resto de código de forma que estará siempre aislado. En RJS los módulos son una extensión del patrón de módulo con la ventaja de no necesitar raelizar referencias a otros módulos globales. Gracias a utilizar RJS vamos a poder cargar módulos no necesariamente en el orden en el que los declaramos y lo más importante van a tener las dependencias organizadas adecuadamente (de hecho en la documentación oficial de RJS se nos informa que los módulos de RJS respetan el formato de los módulos de CommonJS). Por tanto tiene que haber únicamente una definición de módulo por fichero, eso sí, los módulos pueden ser agrupados en grupos optimizados utilizando para ello la herramienta de optimización. 

##Aplicando RJS a una Aplicación de AngularJS

En el fichero *index.html*, tendremos únicamente la línea en la que estamos importando el script de RJS y le diremos que el punto de entrada es en nuestro caso *app/main*: 

{% highlight ruby %}
<script data-main="app/main" src="vendor/scripts/require"></script>
{% endhighlight %}

Después, si vamos al directorio *app* y abrimos el fichero *main* empezaremos teniendo un array de controladores llamado por ejemplo *controllers*. Después, vamos a declarar el path o localización donde estarán nuestros controladores, podremos declararlos en una variable que se llame por ejemplo **ctrllPath** y que estará apuntando/buscando ficheros con el siguiente patrón: 

{% highlight ruby %}
../app/{{ctrName}}{{ctrName}}Controller
{% endhighlight %}

Esto lo hacemos para que dinámicamente nos vaya incluyendo y conformando los nombres de los ficheros en los que estarán definidos los controladores, de esta forma, a la hora de conformar las dependencias de la aplicación (*appDeps*), podremos realizar un bucle para recorrer todos los controladores existentes y que automáticamente nos conforme el identificador del módulo sin tener que escribirlo manualmente. 

A continuación definiremos el objeto *configuration* de RJS que en la documentación oficial de RJS podemos leer que es así: 

{% highlight ruby %}
require.config({
    paths: {
      foo: 'libs/foo-1.1.3'
    }
});
{% endhighlight %}

WIP

Fuentes: 
http://requirejs.org/docs/start.html



