---
layout: post
title:  "Entendiendo el metatag viewport"
date:   2014-01-04 12:11
categories: viewport metatag browseragnostic
---

La versión de [Mobile Firefox (Fennec)][fennec] incorpora ya soporte mejorado para el tag `<meta name = "viewport">`. Las versiones anteriores de Fennec ya daban soporte a las propiedades de este metatag `width`, `height` y `initial-scale` pero todavía tenían problemas con algunas webs pensadas para verse en los navegadores del iPhone y del Android. Con la introducción de este metatag en Fennec, ahora ya están disponibles las mismas propiedades que tiene el navegador del iPhone además de haber cambiado Fennec para que sea capaz de generar páginas webs pensadas para visualizarse en dispositivos móviles incluyendo diferentes resoluciones y diferentes pantallas. 

##Antecedentes

Los navegadores móviles como Fennec renderizan las páginas en una ventana virtual (conocida como `viewport`). Esta ventana es normalmente más ancha que la pantalla de forma que no necesitan estrujar cada página en una ventana de menor espacio (la cual podría fastidiar la visualización de muchos sitios que no estuviesen optimizados para verse en dispositivos móviles). Los usuarios podrán realizar zoom para ver las diferentes áreas de la página. 

El navegador Safari introdujo el metatag `viewport` justo para facilitar a los desarrolladores web el control de la escala y el alto y el ancho de la página. A día de hoy muchos otros navegadores ya utilizan este metatag aunque no es parte del standard Web y la documentación básica al respecto está disponible en la página para desarrolladores de [Apple][appledevelopers]. Sin embargo, los creadores de Fennec tenían que pensar cómo adaptarlo a su standard ya que por ejemplo Apple decía que los parámetros podían ir separados por comas mientras que muchos navegadores utilizan una mezcla de comas, puntos y comas y espacios como elementos de separación. 

##Aspectos básicos de viewport

Una página web optimizada para verse en navegadores de dispositivos móviles utiliza este metatag de la siguiente forma: 

```html
<meta name="viewport" content="width=device-width, user-scalable=no">
```

Como puedes observar, la propiedad `width` controla el espacio que ocupará el `viewport`. Podremos ponerlo a un valor específico de píxels como por ejemplo `width=600` o como está en el ejemplo anterior en el que recibirá el valor que tenga el dispositivo (en escala de pixels CSS al 100%). Además, también tenemos los atributos `height=device-height` como es de esperar. Después, el atributo `initial-scale` controlará el zoom inicial que tendrá la página cuando se carga por primera vez y el atributo/propiedad `user-scalable` controlará el tipo de zoom in y zoom out que podrán aplicar los usuarios a la página. 

##Un pixel no es un pixel directamente

El iPhone y muchos dispositivos Android tienen pantallas desde 3 a 4 pulgadas (7 a 10 cm) con un número de píxels que varía entre 320 y 480 (~160 dpi). En el caso de por ejemplo el navegador Firefox en [Maemo][maemourl] puede tener una pantalla de por ejemplo 480 a 800 píxels (~240 dpi). Debido a esto, la versión de Fennec anterior mostraba algunas páginas con un espacio menor que las páginas reales lo cual produce problemas de usabilidad y lectura en muchas webs optimizadas para ser utilizadas mediante interfaz de toques. Peter-Paul escribió un artículo al respecto llamado [A pixel is not a pixel is not a pixel][peterpaul-apixelisnotapixelisnotapixel] 

El estándar Fennec 1.1 para Maemo utilizará realmente 1.5 píxels en hardware para cada pixel CSS (como ya se hace en los navegadores basados en WebKit). Esto significa que una página con el atributo `initial-scale=1` se verá muy parecida a la página real física en Fennec para Maemo, Mobile Safari para iPhone y el navegador de Android tanto en móviles [HDPI como en MDPI][hdpiandmdpi]. Esto es consistente con la [especificación CSS 2.1][especificacion-css2.1] que dice lo siguiente: 

> Si la densidad del pixel del dispositivo en el que se verá es muy diferente del qeu se ve en una pantalla de un ordenador típico, el `user agent` será el que tenga que realizar un escalado de los píxels. Se recomienda que la unidad del pixel se refiera al número total de pixels en el dispositivo que mejor se aproxime al pixel de referencia. Se recomienda que el pixel de referencia sea el ángulo visual de un pixel en un dispositivo con una densidad de pixel de 96 dpi y a una distancia del lector con una longitud de un brazo aproximada. 

Por tanto, para desarrolladores web esto significa que 320px será el ancho completo en modo *portrait* con el atributo `scale=1` en la mayoría de los dispositivos de forma que las imágenes y las plantillas cuadren de forma adecuada. Pero recuerda que no todos los móviles tienen la misma superficie (size) en sus navegadores web. Además, debes asegurarte que tu página se vea correctamente en modo *landscape* y en dispositivos más grandes como un iPad o tablets Android. 

En pantallas con 240 dpi, las páginas con el atributo `initial-scale=1` tendrán una visualización inicial al 150% de forma correcta tanto en Fennec como en el motor WebKit de Android. Su texto se verá detallado y suave sin embargo las imágenes no se aprovechen de esta ventaja en pantalla completa. Para conseguir imágenes más definidas en este tipo de pantallas, quizás les interese a los desarrolladores web realizar los *designs* de sus imágenes (o todo su *layout*) al 150% de la superficie final que van a ocupar (o también al 200% para dar soporte a los dispositivos con 320 dpi como la pantalla retina del iPhone). Y luego realizar una reducción mediante CSS o mediante la propiedad que ya hemos visto del viewport. 

El ratio por defecto depende de la densidad del *display*. Con menos de 200 dpi, el ratio es 1:0. Con ratios entre 200 y 300 dpi será 1:5. Para *displays* por encima de los 300 dpi, el ratio será `densidad/150dpi`. Hay que recordar que el ratio por defecto solo se tendrá en cuenta cuando el valor del atributo `scale` del viewport esté a 1. En cualquier otro caso, la relación entre los píxels CSS y los píxels del dispositivo dependerán del nivel de zoom. 

##El ancho y alto del viewport

Muchas webs usan `width=320, initial-scale=1` para que se vean bien en el modo *portrait* (cuadro erguido). Como ya hemos dicho antes, esto daba problemas cuando Fennec mostraba estas páginas especialmente en modo *portrait* (apaisado). Para solucionar esto, Fennec 1.1 va a expandir el ancho del viewport si es necesario para rellenar la pantalla a la escala que sea la necesaria. Así se comportará igual que los navegadores de Android y iPhone y será especialmente útil en pantallas grandes como por ejemplo el iPad (consulta el artículo de Allen Pike sobre [Choosing a viewport for iPad sites][allenpike-choosing-a-viewport]). 

En el caso de páginas que definen una escala máxia o inicial, significa que la propiedad/atributo `width` está definida al valor mínimo en el viewport, es decir, por ejemplo si el *layout* necesita al menos 500 píxels de ancho entonces usaremos el viewport así: 

```html
<meta name="viewport" content="width=500, initial-scale=1">
```

En este caso, cuando las pantallas tengan más de 500 píxels de ancho, el navegador expandriá el viewport (en lugar de aplicar un zoom para que se pueda ver completo en la pantalla). Además, Fennec también utiliza el atributo `minimum-scale`, `maximum-scale` y `user-scalable` con valores por defecto similares a los que se usan en el navegador Safari del iPhone. Estas propiedades afectan al ancho y la escala inicial así como también limitan cambios a nivel del zoom que se aplica. 

Los navegadores móviles gestionan los cambios en su orientación de forma diferente entre ellos. Por ejemplo, Safari, al pasar de *portrait* a *landscape* lo que suele realizar es un zoom en lugar de disponer la página como debería ir acorde a una plantilla horizontal. Es por esto que si queremos que los valores de escalado sean coherente en el iPhone, tenemos que utilizar el atributo `maximum-scale` para evitar que Safari realice ese zoom por defecto que puede ocasionar un efecto colateral en el que los usuarios no puedan realizar ampliaciones mediante zoom, por eso tendremos que usar este viewport: 

```html
<meta name="viewport" content="initial-scale=1, maximum-scale=1">
```

Esto ya no es necesario en Fennec ya que cuando el dispositivo cambia su orientación, Fennec actualizará el espacio del viewport, el layout y las propiedades de CSS/JS como por ejemplo `device-width` acorde con las nuevas dimesiones de la pantalla. 

##Anchos y altos típicos para móviles y tablets

Aquí tenéis una [lista bastante extensa sobre qué valores de ancho hay que colocar en el viewport dependiendo del dispositivo][lista-anchos-tipicos]. En esta tabla tenemos información sobre los valores de ancho que hay que definir en nuestro viewport tanto en *portrait* como en *landscape* además de tener el ancho y alto, sistemas operativos y densidad de píxels de diferentes dispositivos. 

##Estándares 

En la actualidad cada vez más se necesita un metatag como `viewport`, esto es así ya que cada vez más navegadores lo utilizan para poder cargar la visualización de las páginas de la mejor forma posible. Estaría muy bien que este metatag se incorporase en el estándar, por lo que habrá que estar atentos al futuro del HTML para ver si finalmente lo termina adoptando. 

Fuente original: [Using the viewport meta tag to control layout on mobile browsers][original-source]

<!-- [id_ref]: URL--> 
[fennec]: https://wiki.mozilla.org/Mobile/Fennec
[appledevelopers]: https://developer.apple.com/library/safari/documentation/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html 
[maemourl]: http://en.wikipedia.org/wiki/Maemo
[peterpaul-apixelisnotapixelisnotapixel]: http://www.quirksmode.org/blog/archives/2010/04/a_pixel_is_not.html  
[hdpiandmdpi]: http://developer.android.com/guide/practices/screens_support.html#range
[especificacion-css2.1]: http://www.w3.org/TR/CSS2/syndata.html#length-units
[allenpike-choosing-a-viewport]: http://www.antipode.ca/2010/choosing-a-viewport-for-ipad-sites/
[lista-anchos-tipicos]: http://i-skool.co.uk/mobile-development/web-design-for-mobiles-and-tablets-viewport-sizes/
[original-source]: https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag
