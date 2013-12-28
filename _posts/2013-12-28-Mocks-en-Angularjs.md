---
layout: post
title:  "Mocks en AngularJS $httpBackend"
date:   2013-12-28 19:53
categories: mocks angularjs httpbackend 
---

El servicio **$httpBackend**, como su nombre indica, proporciona un servicio de bajo nivel equivalente a un *backend* para interactuar con *XMLHttpRequest*. Este servicio te evita los problemas relacionados con las incompatibilidades de los navegadores y los detalles para permitir las peticiones entre dominios diferentes utilizando JSONP. Sobre este servicio está situado el ya conocido **$http**. 

El módulo `ngMock` contiene un servicio con el mismo nombre el cual nos va a permitir realizar simulaciones relacionadas con las llamadas HTTP. Esto es muy práctico en situaciones en las que todavía no tenemos un *backend* y necesitamos simular que una API nos devuelve información. 

Por tanto, lo primero que hay que hacer es cargar el script de `angular-mock.js`.

###Empecemos con la simulación del Backend

En el mundo de Angularjs tenemos el servicio `$resource` para interactuar con interfaces RESTful. Este servicio nos proporciona un nivel más alto de abstracción que el propio servicio `$http` de forma que no tenemos que escribir el código por defecto para realizar las típicas operaciones CRUD (crear, leer, actualizar y borrar). Por tanto la situación es la siguiente: nuestro servicio `$http` va a utilizar `$httpBackend` el cual va a ser el que responda directamente a las peticiones que hagamos a una API en lugar de enviarlas verdaderamente a la API. Veamos un ejemplo por ejemplo en el que capturamos o interceptamos una llamada a una API: 

{% highlight ruby %}
$httpBackend.whenGET('/api/101/activity/').respond(function(method, url, data){
  return [200,[{workoutDate: new Date(), activityType: 'Running', duration: 22,  calories: 200, distance: 2.2}]];
});
{% endhighlight %}

####Secciones interesantes

Vamos a recordar algunos aspectos interesantes. La sección de código `run`, es el fragmento de código por el que se pasa cuando la aplicación de Angularjs está arrancando. Y es en esta sección donde se puede configurar todas las sustituciones a llamadas HTTP que queramos. Incluso podemos utilizar diferentes secciones dependiendo del tipo de llamada como por ejemplo: `whenGet`, `whenPOST`, `whenPUT` o utilizar tus propios métodos. 

Ahora toca analizar el método `when<method>(method, url, data, headers)`. Como ves tenemos cuatro parámetros para definir o simular la interacción con nuestro *backend*. De hecho las URLs se pueden definir como expresiones regulares, lo cual nos puede facilitar la captura de varias URLs bajo un mismo método, veamos un ejemplo:

{% highlight ruby %}
$httpBackend.whenPOST(/api\/(\d+)\/activity/).respond(function(method, url, data){
  data = JSON.parse(data);
  var searchResult = ulr.match(/api\/(d+)\/activity/);
  var userId = parseInt(searchResult[1]);
  var savedActivity = activitiesRepository.save(data);
  return [200, savedActivity];
})
{% endhighlight %}

Ahora bien, supongo que ya te estarás preguntando cómo puedes hacer para que una petición si que llegue hasta la API en lugar de simular la respuesta con httpBackend. Esto puede resultar útil porque a medida que el *backend developer* vaya completando funcionalidades en la API, podemos ir incorporándolas en nuestro *workflow*. Para ello, tenemos que utilizar el método `passThrough` en lugar de declarar una función como *callback*: 

{% highlight ruby %}
$httpBackend.whenGET(/\partials\//).passThrough();
{% endhighlight %}

###Utiliza un almacenamiento local

Una vez que podemos interceptar las llamadas a nuestra API, lo siguiente que podemos necesitar es realizar un almacenamiento de los datos. Para ello, podemos hacer uso del objeto `Repository`, el cual proporciona métodos como por ejemplo `save`, `delete`, `find`, etc. La gestión de los datos la hace automáticamente así que no tenemos que preocuparnos por asignar identificadores y todo este tipo de gestiones. Algunos ejemplos de uso de este repositorio son:

{% highlight ruby %}
var userRepository = new utils.Repository();
var activitiesRepository = new utils.Repository();
var savedActivity = activitiesRepository.save(data);
var activities = activitiesRepository.findAll({userId: userId});
{% endhighlight %}

Fuente: [CodeOrbits] (http://www.codeorbits.com/blog/2013/12/20/rapid-angularjs-prototyping-without-real-backend/?goback=%2Egde_4896676_member_5822732500522262528#%21)


