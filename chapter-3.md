## Capítulo 3. Comunicación con un servidor back-end

Más a menudo las aplicaciones no web necesitan comunicarse con un almacén persistente para extraer y manipular la data. Esto es especialmente cierto para las aplicaciones tipo CRUD donde la edición de data es una parte esencial.

AngularJS esta bien equipado para comunicarse con varios back-ends usando **XMLHttpRequest (XHR)** y solicitudes **JSONP**. Él tiene un servicio de propósito general `$http` para emitir llamadas XHR y JSONP como también tiene un servicio especializado `$resource` para apuntar fácilmente a puntos de entradas (endpoints) **RESTful**.    

Este este capítulo, nosotros vamos a examinar diferentes APIs y técnicas para comunicarnos con varios back-ends. En particular, nosotros vamos a aprender cómo:

- Hacer llamadas XHR usando el servicio `$http` y probar el código confiando en `$http`.

- Trabajar efectivamente con solicitudes asíncronas, usando la API promesa (Promesa API) expuesta por AngularJS en el servicio `$q`.

- Hablar fácilmente a los puntos de entradas (endpoints) RESTful usando la dedicada fábrica (factory) `$resource`.

- Construir una personalizada, API como `$resource` adaptada a las necesidades de nuestro back-end.
  
### Haciendo solicitudes XHR y JSON con $http

El servicio `$http` es una API universal fundamental, para hacer llamadas XHR y JSONP. La API en si esta bien diseñada y es fácil de utilizar, como vamos a ver en breve, Antes de profundizar en detalles de la API `$http`, nosotros necesitamos aprender un poco más sobre el modelos de datos de nuestra aplicación de ejemplo SCRUM para que nosotros podamos seguir significativamente los ejemplos.

## Familizializarce com el modelo de datos y la URLs MongoLab

El modelo de data del la aplicación de ejemplo SCRUM es bastante simple y puede ser ilustrado en el siguiente diagrama:

![chapter-3-1](/img/chapter-3-1.png)

Hay cinco diferentes colecciones MongoDB que cargan datos de los usuarios, proyectos, y artefactos relacionados con el proyecto. Toda la data es accessible a través de la interfaz RESTful expuesta por MongoLab. La API REST puede ser invocada apuntando a las URLs con un patrón muy bien definido:

```
https://api.mongolab.com/api/1/databases/[DB name]/collections/[collection name]/[item id]?apiKey=[secret key]
```

> `NOTA` Todas la llamadas a la API REST que apuntan a la base de datos alojadas en MongoLab necesitan incluir un parámetro llamado `apiKey`. El parámetro `apiKey`, con el valor único de una cuenta individual, es necesario para autorizar las llamadas a la API REST MongoLab. La completa descripción de la API REST expuesta por MongoLab puede ser consultada en https://support.mongolab.com/entries/20433053-rest-api-for-mongodb.

### Vista rapida a las APIs $http

Hacer llamadas XHR y JSONP usando el servicio `$http` es sencillo. Vamos a considerar un ejemplo de extracción de contenido JSON con una solicitud GET.

```
var futureResponse = $http.get('data.json');

futureResponse.success(function (data, status, headers, config) {
  $scope.data = data;
});

futureResponse.error(function (data, status, headers, config) {
  throw new Error('Something went wrong...');
});
```

Primero que todo nosotros podemos ver que hay un método dedicado para las solicitudes GET XHR problemáticas. También hay métodos equivalentes para otros tipos de solicitudes XHR:  

- GET: $http.get(url, config)
- POST: $http.post(url, data, config)
- PUT: $http.put(url, data, config)
- DELETE: $http.delete(url, config)
- HEAD: $http.head


Es también posible desencadenar una solicitud JSONP con `$http.jsonp(url, config)`.

Los parámetros aceptados por los métodos `$http` variar ligeramente dependiendo del método HTTP usado. Para las llamadas que pueden transportar datos en su cuerpo (POST y PUT) la firma del método es la siguiente:

- **url**: la URL a apuntar con una llamada.
- **data**: la data a ser enviada con el cuerpo de una solicitud.
- **config**: un objeto javascript que contiene una opcional configuración adicional que influyen en la petición y la respuesta.   
  
Para los métodos restantes (GET, DELETE, HEAD, JSONP) donde no hay datos que se enviarán con el cuerpo de la solicitud, la firma llegar a ser simple y es reducida a dos parámetros solamente: `url` y `config`  

El objeto devuelto por los métodos `$http` nos permite registrar retrollamadas de éxito y error.  

### La configuración del objeto

El objeto de configuración JavaScript puede mantener diferentes opciones que influyen en la solicitud, la respuesta y la data siendo transmitida. El objeto de configuración puede tener las siguientes propiedades (entre otras).

- **method**: método HTTP a emitir.
- **url**: a URL a apuntar con una llamada.
- **params**: parámetros para ser añadidas a la cadena de la solicitud URL
- **headers**: cabeceras adicionales para ser añadidas a la solicitud.
- **timeout**: tiempo de espera (en milisegundos) tras lo cual una solicitud XHR será dada de baja.
- **cache**: habilita el almacenamiento en caché para las solicitudes GET XHR.
- **transformRequest**, **transformResponse**: funciones de transformación que nos permiten el preproceso y postproceso de los datos intercambiados con un back-end

Usted probablemente esté un poco sorprendido de ver method y url entre las opciones de configuración ya que estos parámetros pueden ser suministrados como parte de una firma de los métodos de `$http`. Resulta que el mismo `$http` es una función que puede ser invocada de forma genérica:

```
$http(configObject);
```

La forma genérica puede ser útil para algunos casos donde AngularJS no provee un método directo (por ejemplo para solicitudes PATCH o OPTIONS). En general nosotros encontramos que los métodos directos resultan en una más concisa y fácil forma de leer código, y nosotros recomendamos el uso de esta forma sobre la genérica siempre que sea posible.

### Conversión de la data solicitada

Los métodos `$http.post` y `$http.put` aceptan caulquier objeto JavaScript (o una cadena) como su parámetro de datos. Si la `data` es un objeto JavaScript esta será por defecto convertida a una cadena JSON.

> `TIP` El mecanismo de conversión de data a JSON por defecto ignora todas las propiedades que comienzan con un signo dólar `$`. En general, las propiedades que comienzan con `$` son consideradas “privadas” en AngularJS. Esto puede ser problemático para algunos back-ends  al que nosotros necesitamos enviar propiedades con el `$` (por ejemplo, MongoDB). La solución es convertir la data manualmente (usando el método `JSON.stringify`, por ejemplo).

Nosotros podemos ver la conversión de la data en acción mediante la emisión de una solicitud POST para crear un nuevo usuario en MongoLab:

```
var userToAdd = {
  name:'AngularJS Superhero',
  email:'superhero@angularjs.org'
};
$http.post(
  'https://api.mongolab.com/api/1/databases/ascrum/collections/users',
  userToAdd,
  {
    params:{
      apiKey:'4fb51e55e4b02e56a67b0b66'
    }
  }
);
```

Este ejemplo también ilustra como los parámetros de cadena de la consulta HTTP (aquí: `apiKey`) pueden ser añadidos a la URL.

### Tratar con respuestas HTTP

Una solicitud puede ser cualquiera de las dos exitosa o fallida (`success` y `error`), y AngularJS provee dos métodos para registrar retrollamadas para hacer frente a los dos resultados:  success y error. Ambos métodos aceptan una función retrollamada que será con los siguiente parámetros:

- **data**: La data de la respuesta actual.
- **status**: El status HTTP de la respuesta.
- **headers**: Una función que da acceso a las cabeceras de la respuesta HTTP.  
- **config**: El objeto de configuración que fue suministrado cuando la solicitud fue desencadenada.    


> `NOTA` AngularJS invocara la retrollamada success para las respuestas HTTP con el estatus que oscila de 200 a 299. Respuestas con un status fuera de este rango desencadenará la retrollamada error. Las respuestas de redirección (códigos de estado HTTP 3xx) son automáticamente seguidas por un navegador.

Ambas retrollamadas success y error son opcionales. Si nosotros no registramos ninguna retrollamada una respuesta será ignorada.

### La conversión de datos de respuesta

Al igual que con las conversiones de datos de solicitudes, el servicio `$http` tratara de convertir las respuestas que contienen una cadena JSON en un objeto JavaScript. Esta conversión sucede antes de invocar las retrollamadas success o error. El comportamiento de conversión por defecto puede ser personalizado.

> `NOTA`: En la actual versión de AngularJS el servicio $http intentará realizar la conversión de una cadena JSON a un objeto JavaScript en cualquier respuesta que se parezca a JSON (es decir, si comienza con { o [  y terminar con ] o }).

### Tratar con las restricciones de la política del mismo origen

Los navegadores web hacen cumplir la **política de seguridad del mismo origen**. Esta política autoriza interacciones XHR sólo con recursos procedentes de la misma fuente (definida como una combinación de un protocolo, anfitrión, y su puerto) e impone restricciones a las interacciones con los recursos de "extranjeros".

Como desarrolladores web, nosotros necesitamos balancear constantemente las consideraciones de seguridad con requerimientos funcionales para agregar datos desde múltiples fuentes. De hecho, a menudo es deseable extraer datos desde servicios de terceros y presentar esos datos en nuestras aplicaciones webs. Desafortunadamente, las solicitudes XHR  no pueden llegar fácilmente a los servidores fuera del dominio de origen a menos que nosotros juguemos con algunos trucos.   

Hay varias técnicas para acceder a los datos expuestos por servidores externos: **JSON with padding (JSONP)** y **Cross-origin resource sharing (CORS)** son probablemente los más populares en la web moderna. Esta sección muestra como AngularJS nos ayuda aplicar esas técnicas en la práctica.     

### Superar las restricciones de la política del mismo origen con JSONP

Es un truco usar JSONP ya que nos permite extraer los datos pasando las restricciones de la política del mismo origen. Se basa en el hecho que los navegadores pueden colocar libremente JavaScript desde servidores extranjeros mediante el uso de la etiqueta `<script>`.

Las llamadas JSONP no desencadena solicitudes XHR en su lugar generan una etiqueta `<script>` cuyo origen apunta a un recurso externo. Tan pronto como una etiqueta script es generada esta aparece en el DOM, el navegador realiza su trabajo y llama a el servidor. El servidor rellena la respuesta con una invocación de la función (por lo tanto el "relleno" {"padding"}  en el nombre de la técnica JSONP ) en el contexto de nuestra aplicación web.     

Vamos a examinar un ejemplo de una solicitud y respuesta JSONP para ver como funciona en la práctica. Primero que todo nosotros necesitamos invocar una solicitud JSONP:

```
$http
.jsonp('http://angularjs.org/greet.php?callback=JSON_CALLBACK', {
  params:{
    name:'World'
  }
}).success(function (data) {
  $scope.greeting = data;
});
```

Al invocar el método `$http.jsonp` AngularJS dinámicamente creará un nuevo elemento DOM `<script>` como:   

```
<script type="text/javascript" src="http://angularjs.org/greet.php?callback=angular.callbacks._k&name=World"></script>
```

Tan pronto como esta etiqueta es ajustada a el DOM el navegador solicitará la URL especificada en el atributo src. La respuesta, a su llegada, tendrá un cuerpo siguiendo un patrón similar:

```
angular.callbacks._k ({"name":"World","salutation":"Hello","greeting":"Hello World!"});
```

Una respuesta JSONP se parece a una regular llamada de una función JavaScript y en efecto es exactamente lo que es. AngularJS genera la función `angular.callbacks._k` detras de escenas. Esta función, cuando es invocada, desencadenará una invocación de la retrollamada success. La URL suministrada a la función `$http.jsonp` deberá contener el parámetro solicitado `JSON_CALLBACK`. AngularJs convertirá esta cadena en una función generada dinámicamente.

> `TIP` Los nombre de las retrollamadas JSONP generadas por AngularJs tendrá una forma de: `angular.callbacks._[variable]`. Cerciorarse que su back-end pueda aceptar nombres de retrollamadas que contenga puntos.

### Limitaciones de JSONP

JSONP es inteligente y útil para trabajar alrededor de las restricciones de la política del mismo origen pero tiene varias limitaciones. Primeramente, solamente las solicitudes GET HTTP pueden ser emitidas usando técnicas JSONP. El manejo de errores también es muy problemático, ya que los navegadores no exponen el estatus de la respuesta HTTP para la etiqueta `<script>` de nuevo a JavaScript. En la práctica significa que es bastante difícil reportar de forma fiable el estatus HTTP de los errores e invocar las retrollamadas de error.  

Además JSONP expone nuestra aplicación web a varias amenazas de seguridad. Además de el bien conocido ataque XSS, probablemente el mas grande error es que el servidor puede generar arbitrariamente cualquier JavaScript en la respuesta JSONP. Este JavaScript será cargado en el navegador y ejecutado en el contexto de la sección del usuario. Un servidor configurado de una forma maliciosa puede ejecutar scripts indeseados causando diferentes daños y perjuicios, que van desde simplemente romper la pagina a robar datos sensibles.  Como tal, nosotros debemos tener mucho cuidado mientras seleccionamos servicios dirigidos por solicitudes JSONP y solo usar servidores de confianza.  

### Superar las restricciones de la política del mismo origen con CORS

`Cross-origin resource sharing (CORS)` es una especificación que anima a solventar el mismo problema como JSONP en una forma segura, confiable y estándar. La especificación CORS estructurada en el objeto `XMLHttpRequest` nos permite solicitudes AJAX entre dominios de una manera bien definida y controlada.

Toda la idea detrás de CORS es que un navegador y un servidor extranjero necesitan coordinarse (mediante el envío de los apropiados encabezados de solicitud y respuesta) para condicionalmente permitir solicitudes entre dominios. Como tal, un servidor foraneo necesita ser configurado apropiadamente. Los navegadores deben poder enviar las solicitudes apropiadas, cabeceras, y interpretar las respuestas del servidor para completar satisfactoriamente las solicitudes entre dominios.

> `NOTA`  Un servidor foraneo debe ser configurado apropiadamente para participar una conversación CORS. Aquellos que necesiten configurar servidores para aceptar CORS HTTP puede encontrar más información en http://www.html5rocks.com/en/tutorials/cors/. Aquí vamos a enfocarnos en el rol del navegador en toda la comunicación.

Las solicitudes CORS se dividen más o menos en "simples" y las "no simples". Las solicitudes GET, POST y HEAD son consideradas como “simples” (pero solo cuando envía un subconjunto de cabeceras permitidas). Usar otros verbos HTTP o  solicitar cabeceras fuera del conjunto permitido forzará al navegador a emitir una solicitud CORS “no simple”.  

> `NOTA` La mayoría de los modernos navegadores soportan comunicación CORS fuera de la caja. Internet Explorer en la versión 8 y 9  habilita el soporte para CORS sólo con el no estándar objeto XDomainRequest. Debido a las limitaciones de la implementación de XDomainRequest  específica de IE AngularJS no provee soporte para él. En consecuencia, las solicitudes CORS no son soportadas con el servicio $htttp en IE 8 y 9.

Con solicitudes no simples, los navegadores están obligados a enviar una solicitud sondeo (**prevuelo**) OPTION y esperar por la aprobación de los servidores antes de emitir la solicitud primaria. Esto es a menudo confuso, ya que una inspección cercana de el tráfico HTTP revela misteriosas solicitudes OPTIONS. Nosotros podemos ver esas solicitudes tratando de llamar la API REST MongoLab directamente desde el navegador. Como un ejemplo, vamos a inspeccionar la comunicación HTTP mientras borramos un usuario:


```
$http.delete('https://api.mongolab.com/api/1/databases/ascrum/collections/users/' + userId,
  {
    params:{
      apiKey:'4fb51e55e4b02e56a67b0b66'
    }
  }
);
```

Nosotros podemos ver dos solicitudes (**OPTIONS** y **DELETE**) apuntando a la misma URL.

![chapter-3-2](/img/chapter-3-2.png)

La respuesta que viene de el servidor MongoLab incluye cabeceras que hace la final solicitud DELETE posible.

![chapter-3-3](/img/chapter-3-3.png)

Los servidores MongoLab están bien configurados para enviar cabeceras apropiadas en respuesta de la solicitud CORS. Si su servidor no esta apropiadamente configurado la solicitud OPTIONS fallará y el objetivo de la solicitud no será ejecutado.

> `TIP` No se sorprenda al ver solicitudes OPTIONS; esto es tan sólo como funciona el mecanismo de apretón de manos CORS. A falta de las solicitudes OPTIONS lo más probable indicará que el servidor no esta bien configurado.

### Proxys del lado del servidor

JSONP no es una técnica ideal para hacer solicitudes de origen cruzado. La especificación CORS hace la situación mejor, pero aún requiere configuración adicional en el lado del servidor y un navegador que soporte el estándar.

Si usted no quiere usar las técnicas CORS o JSONP, entonces siempre esta la opción de evitar todos los problemas de las solicitudes entre dominios. Nosotros podemos lograr esto configurando un servidor local como un proxy a uno extranjero. Mediante la aplicación de la correcta configuración del servidor nosotros podemos representar solicitudes entre dominios a través de nuestro servidor, y por lo tanto tener el navegador apuntado solo a nuestros servidores. Esta técnica funciona en todos los navegadores, no requiere el prevuelo de solicitudes OPTIONS. Además, no nos expone a riesgos de seguridad adicionales. La desventaja de este enfoque es que tenemos que configurar el servidor como corresponde.

> `NOTA` La aplicación de ejemplo SCRUM descrita en este libro depende de el servidor node.js configurado de una manera que sus proxys llaman a la API REST MongLab.

### La API promesa con $q

Los programadores están acostumbrados a el modelo de programación asincrono. Ambos entornos de ejecución node.js y el navegador están llenos de eventos asíncronos: respuestas XHR, eventos DOM, IO y tiempos de espera (timeouts), los cuales pueden ser desencadenados en cualquier momento y en orden aleatorio. Aunque, todos estamos acostumbrados a lidiar con la naturaleza asíncrona de el entorno de ejecución, la verdad es que la programación asíncrona puede ser confusa, especialmente cuando se trata de sincronizar múltiples eventos asíncronos.  

En el mundo síncrono encadenar las llamadas de funciones (invocar una función con un resultado de otra función) y manejar excepciones (con try/catch) es simple. En el mundo asíncrono, no podemos simplemente encadenar las llamadas de funciones; nosotros necesitamos confiar en las retrollamadas. Usar retrollamadas están bien cuando se trata de un solo evento asíncrono, pero las cosas empiezan a complicarse tan pronto como tenemos que coordinar varios eventos asincrónicos. Manejar situaciones excepcionales es particularmente difícil en este caso.  

Para hacer fácil la programación asíncrona la API Promesa fue recientemente adoptada por varias librerías JavaScript populares. El concepto detrás de la API Promesa no es nuevo, y fue propuesto a finales de los años setenta, pero sólo recientemente esos conceptos entraron en la corriente principal de la programación JavaScript.  

> `TIP` La idea principal detrás de la API Promesa es de brindar al mundo asíncrono la misma facilidad de encadenar las llamadas de funciones y manejo de errores tal como nosotros podemos disfrutar en el mundo de la programación síncrona.

AngularJS viene con el servicio $q una ligera implementación de la API Promesa. Un número de servicios AngularJS (más notablemente $http, pero también $tiemout y otros) dependen en gran medida del estilo de las APIs promise. Así que tenemos que familiarizarnos con $q con el fin de utilizar esos servicios eficazmente.

NOTA: El servicio $q esta inspirado por la librería de la API Promise Q de Kris Kowal's  (https://github.com/kriskowal/q). Es posible que desee echar un vistazo a esta biblioteca para obtener una mejor comprensión o conceptos de promise y comparar la implementación ligera de AngularJS con la plena librería de la API Promise.  
Trabajando con promesas y el servicio $q

Para conocer la relativamente pequeña API expuesta por el servicio $q, vamos a utilizar ejemplos de la vida real, sólo para demostrar que la API Promise se puede aplicar a cualquier evento asíncrono, y no sólo a las llamadas XHR.
Aprendiendo lo básico del servicio $q

Vamos a imaginarnos que nosotros queremos ordenar pizza a través del telefono y ya la han entregado en nuestra casa. El resultado de nuestra orden de pizza puede ser cualquiera de estos dos: pedido entregado o una llamada indicando problemas con nuestra order. Mientras que ordenar una pizza es sólo una cuestión de una breve llamada telefónica, la entrega real ( el cumplimiento de la orden) toma algún tiempo, y es asíncrona.

Para obtener la sensación de la API de Promise, vamos a echar un vistazo a la orden de pizza, y su entrega exitosa, modelada usando el servicio $q, En primer lugar, vamos a definir una persona que pueda consumir una pizza o simplemente se decepcione cuando una orden no es entregada:

var Person = function (name, $log) {

	this.eat = function (food) {
		$log.info(name + " is eating delicious " + food);
	};
	
	this.beHungry = function (reason) {
		$log.warn(name + " is hungry because: " + reason);
	}
	
};

El constructor Person definido anteriormente puede ser usado para producir un objeto que contenga los métodos eat y beHungry. Nosotros vamos a usar esos métodos como las retrollamadas de error y success, respectivamente.

Ahora, vamos a modelar un pedido de pizza y el proceso de cumplimiento, escrito como una prueba Jasmine:

it('should illustrate basic usage of $q', function () {

	var pizzaOrderFulfillment = $q.defer();
	var pizzaDelivered = pizzaOrderFulfillment.promise;
	
	pizzaDelivered.then(pawel.eat, pawel.beHungry);
	
	pizzaOrderFulfillment.resolve('Margherita');
	$rootScope.$digest();

	expect($log.info.logs).toContain(['Pawel is eating delicious Margherita']);

});

Las pruebas unitarias comienzan con la llamada al método $q.defer que devuelve un objeto diferido o aplazado. Conceptualmente representa una tarea que se completara (o fallara en el futuro). El objeto diferido tiene dos roles:


- Contiene un objeto prometido (en la propiedad promise). Las promesas (Promises) son marcadores de posición para los resultados futuros (éxito o fracaso) de una tarea aplazada.

- Expone métodos para desencadenar la finalización de futuras tareas (resolve) o en caso de fallo (reject).

Siempre hay dos jugadores en la API Promise: uno que controla la ejecución de las futuras tareas (pueden invocar métodos en los objetos diferidos) y el otro que depende de los resultados de la ejecución de tareas futuras (se aferra a los resultados prometidos).  

NOTA: El objeto diferido representa una tarea que se completara o fallara en el futuro. Un objeto prometido es un marcador de posición para los futuros resultados de esta tarea finalizada.

La entidad que controla la tarea futura (en nuestro ejemplo sería un restaurante) expone un objeto prometido (pizzaOrderFulfillment.promise) a entidades que estan interesadas en el resultado de la tarea. En nuestro ejemplo, Pawel esta interesado en lo ordenado y puede expresar su interés registrando retrollamadas en el objeto promise. Para registrar una retrollamada del entonces metodo usado (successCallBack, errorCallBack). Este método acepta funciones de retrollamada que serán llamadas con el resultado de una tarea futura (en caso de una retrollamada success) o una razón de fallo (en caso de la retrollamada error). La retrollamada de error es opcional y puede ser omitida. Si la retrollamada de error es omitida y una futura tarea falla, esta falla será silenciosamente ignorada.

Para señalar la finalización de la tarea futura el método resolve debe ser llamado en el objeto diferido. El argumento pasado a el método resolve será usado como un valor previsto para la retrollamada success. Luego de que es llamada una retrollamada sucess una tarea futura es completada y la promesa es resolvida (cumplida). Similarmente, la llamada al método reject desencadenará la invocación de la retrollamada error y la promesa de rechazo.

    
NOTA: En el ejemplo de prueba hay una llamada misteriosa al método $rootScope.$digest(). En AngularJS los resultados de resolución promesa (o rechazo) se propagan como parte del ciclo $digest. Usted puede ver el capitulo 11, Escribir Aplicaciones Web AngularJS de Buen Rendimiento, para aprender más acerca del funcionamiento interno y el ciclo $digest.


Las promesas (Promises) son objetos JavaScript de primera clase.

A primera vista puede parecer que la API Promise añade complejidad innecesaria. Pero para apreciar el verdadero poder de la promesas nosotros necesitamos ver más ejemplos. En primer lugar tenemos que darnos cuenta de que las promesas son objetos JavaScript de primera clase. Podemos pasar a su alrededor como argumentos y devolverlos de llamadas a funciones. Esto nos permite encapsular fácilmente operaciones asincrónicas como servicios. Por ejemplo, imaginemos un servicio de restaurante simplificado:

var Restaurant = function ($q, $rootScope) {
	var currentOrder;
	this.takeOrder = function (orderedItems) {
		currentOrder = {
			deferred:$q.defer(),
			items:orderedItems
		};
		return currentOrder.deferred.promise;
	};
	
	this.deliverOrder = function() {
		currentOrder.deferred.resolve(currentOrder.items);
		$rootScope.$digest();
	};
	this.problemWithOrder = function(reason) {
		currentOrder.deferred.reject(reason);
		$rootScope.$digest();
	};
	
};

Ahora el servicio restaurant encapsula tareas asíncronas y sólo retorna una promesa de su método takeOrder. La promesa retornada puede ser entonces utilizada por los clientes del restaurante para aferrarse a los resultados prometidos y ser notificados cuando los resultados estén disponibles.


Como un ejemplo de esta recién creada API en acción, vamos a escribir código que ilustra las promesas rechazadas y retrollamadas de error siendo invocadas.


it('should illustrate promise rejection', function () {
	pizzaPit = new Restaurant($q, $rootScope);

	var pizzaDelivered = pizzaPit.takeOrder('Capricciosa');

	pizzaDelivered.then(pawel.eat, pawel.beHungry);

	pizzaPit.problemWithOrder('no Capricciosa, only Margherita left');

	expect($log.warn.logs).toContain(['Pawel is hungry because: no Capricciosa, only Margherita left']);

});

Agregando retrollamadas

Un objeto prometido puede ser usado para registrar múltiples retrollamadas. Para ver esto en práctica vamos a imaginar que ambos autores de este libro están ordenando una pizza y como tal ambos están interesados en la orden emitida:

it('should allow callbacks aggregation', function () {
	var pizzaDelivered = pizzaPit.takeOrder('Margherita');
	pizzaDelivered.then(pawel.eat, pawel.beHungry);
	pizzaDelivered.then(pete.eat, pete.beHungry);
	pizzaPit.deliverOrder();
	expect($log.info.logs).toContain(['Pawel is eating delicious Margherita']);

	expect($log.info.logs).toContain(['Peter is eating delicious  Margherita']);

});

Aqui multiples retrollamadas estan registradas y todas ellas están involucradas en la resolución de una promesa. Similarmente, el rechazo de la promesa invocara todas las retrollamadas de error registradas.

Ciclo de vida de las promesas y retrollamadas registradas

Una promesa que se resolvió o se rechaza una vez no puede cambiar su estado. Solo hay un solo chance para proporcionar los resultados prometidos. En otras palabras, no es posible:

-- Resolver una promesa rechazada.
-- Resolver una promesa que ya se resolvió con un resultado diferente.
-- Rechazar una promesa resuelta.
-- Rechazar una promesa rechazada con un motivo de rechazo diferente.

Esas reglas son bastante intuitivas. Por ejemplo, no tendría mucho sentido si pudiéramos ser llamados de nuevo con la información de que hay problemas con la entrega de la orden después que nuestra pizza fue entregada con éxito (y probablemente comida!).

NOTA: Cualquier retrollamada registrada después de que la promesa fue resuelta (o rechazada) se resolverá (o rechazará) con el mismo resultado (o causa de fallo) como al inicio.
Encadenando acciones asincronas

Mientras que agregar retrollamadas es agradable, el verdadero poder de la API Promise radica en su habilidad de imitar invocaciones de funciones sincronas en el mundo asíncrono.

Continuando con nuestro ejemplo de pizza, vamos a imaginar que esta vez nosotros somos invitados por nuestros amigos para una pizza. Nuestros anfitriones ordenarán una pizza y tras arribar la orden ellos agradablemente la cortaron y la sirvieron. Aquí hay una cadena de eventos asíncronos: En primer lugar, una pizza necesita ser entregada, y sólo entonces es preparada para servir. También hay dos promesas que necesitan ser resolvidas antes de que nosotros podamos disfrutar de una comida: un restaurante esta prometiendo una entrega y nuestros anfitriones prometen que la pizza entregada será cortada y servida. Vamos a ver el modelado de código de esta situación:




it('should illustrate successful promise chaining', function () {
	
	var slice = function(pizza) {
		return "sliced "+pizza;
	};

	pizzaPit.takeOrder('Margherita').then(slice).then(pawel.eat);
	pizzaPit.deliverOrder();
	
	expect($log.info.logs).toContain(['Pawel is eating delicious sliced Margherita']);
 
});

En el ejemplo anterior, nosotros podemos ver una cadena de promesas (llamadas a el método  then).  Esta construcción se asemeja estrechamente al código síncrono:
    
	pawel.eat(slice(pizzaPit.takeOrder('Margherita')));

NOTA: El encadenado de promesas es posible sólo cuando el método then retorna una nueva promesa. La promesa retornada será resuelta con el resultado de el valor devuelto por la retrollamada.

Lo que es aún más impresionante es lo fácil que es hacer frente a las condiciones de error. Vamos a echar un vistazo al ejemplo de la propagación de la falta de una persona sosteniendo una promesa:

it('should illustrate promise rejection in chain', function () {

	pizzaPit.takeOrder('Capricciosa').then(slice).then(pawel.eat,pawel.beHungry);
	pizzaPit.problemWithOrder('no Capricciosa, only Margherita left');
	expect($log.warn.logs).toContain(['Pawel is hungry because: noCapricciosa, only Margherita left']);

});





Aquí el resultado de rechazo del restaurante se propaga hasta la persona interesada en el resultado final. Esto es exactamente cómo funciona el manejo de excepciones en el mundo síncrono: Una excepción es lanzada, burbujeará hasta un primer bloque de captura.

En la API Promise las retrollamadas de error actúan como bloques de captura, y como con los bloques de capturas estándar nosotros tenemos varias opciones para manejar situaciones excepcionales. Tenemos la posibilidad de:  

-- Recuperar (un valor devuelto de un bloque de captura).
-- Propagar una falla (relanzar una excepción).

Con la API Promise es fácil simular una recuperación en un bloque de captura. Como un ejemplo, vamos asumir que nuestros anfitriones tendrán un esfuerzo de ordenar otra pizza si la deseada no está disponible.

it('should illustrate recovery from promise rejection', function () {
	
	var retry = function(reason) {
	
		return pizzaPit.takeOrder('Margherita').then(slice);
	
	};

	pizzaPit.takeOrder('Capricciosa')
		.then(slice, retry)
		.then(pawel.eat, pawel.beHungry);

	pizzaPit.problemWithOrder('no Capricciosa, only Margherita left');

	pizzaPit.deliverOrder();

	expect($log.info.logs).toContain(['Pawel is eating delicious sliced Margherita']);

});

Nosotros podemos retornar una nueva promesa desde una retrollamada de error. La promesa retornada será parte de la cadena de resolución, y al el final el cliente no notará que algo salió mal. Este es un patrón muy poderoso que puede ser aplicado a cualquier escenario que requiera reintentos. Nosotros vamos a usar este enfoque en el Capítulo 7, Asegurando Su Aplicación, para implementar controles de seguridad.



El otro escenario que nosotros deberíamos considerar es el re-lanzamiento de excepciones ya que podría suceder que la recuperación no sea posible. En un caso tal la única opción es desencadenar otro error y el servicio $q tiene un método dedicado ($.reject) para este propósito:

it('should illustrate explicit rejection in chain', function () {

	var explain = function(reason) {

		return $q.reject('ordered pizza not available');

	};

	pizzaPit.takeOrder('Capricciosa')
		.then(slice, explain)
	.then(pawel.eat, pawel.beHungry);
	
	pizzaPit.problemWithOrder('no Capricciosa, only Margherita left');
	
	expect($log.warn.logs).toContain(['Pawel is hungry because: ordered pizza not available']);

});

El metodo $q.reject es un equivalente a lanzar una excepción en el mundo asíncrono. Este método devuelve una nueva promesa que es rechazada con una razón especificada como un argumento en la llamada al método $q.reject.

Más sobre $q

El servicio $q tiene dos métodos útiles adicionales: $q.all y $q.when.
Agregando promesas

El metodo $q.all hace posible empezar múltiples tareas asíncronas y ser notificados sólo cuando todas las tareas estan completas. El efectivamente agrega promesas desde varias acciones asincronas, y retorna un sola, promesa combinada que puede actuar como un punto de unión.

Para ilustrar la utilidad del método $q.all, vamos a considerar un ejemplo para ordenar comida desde múltiples restaurantes. A nosotros no gustaría esperar por ambas órdenes antes de  servir toda la comida.  

it('should illustrate promise aggregation', function () {

	var ordersDelivered = $q.all([
		pizzaPit.takeOrder('Pepperoni'),
		saladBar.takeOrder('Fresh salad')
	]);
	ordersDelivered.then(pawel.eat);

	pizzaPit.deliverOrder();

	saladBar.deliverOrder();

	expect($log.info.logs).toContain(['Pawel is eating delicious Pepperoni,Fresh salad']);

});

El metodo $q.all acepta un array de promesas como su argumento, y devuelve una promesa agregada. La promesa agregada será resolvida solo luego de que todas las promesas individuales sean resolvidas. Si, por otra parte, una de las acciones individuales falla la promesa agregada será rechazada también:

 it('should illustrate promise aggregation when one of the promises fail', function () {
	var ordersDelivered = $q.all([
		pizzaPit.takeOrder('Pepperoni'),
		saladBar.takeOrder('Fresh salad')
	]);
	ordersDelivered.then(pawel.eat, pawel.beHungry);

	pizzaPit.deliverOrder();

	saladBar.problemWithOrder('no fresh lettuce');

	expect($log.warn.logs).toContain(['Pawel is hungry because: no fresh lettuce']);

});

Las promesas agregada es rechazada con la misma razón que la promesa individual fue rechazada.

Envasar valores como promesas

A veces podemos encontrarnos en una situación en la que la misma API necesita trabajar con los resultados obtenidos de las acciones síncronas y asíncronas. En este caso, a menudo es más fácil de tratar todos los resultados en forma asincrónica.

NOTA: El método $q.when hace posible envolver un objeto JavaScript como una promesa.

Continuando con nuestro ejemplo “ensalada y pizza”, podemos imaginar que la ensalada está lista (una acción síncrona) pero una pizza necesita ser ordenada y entregada (acciones asincronas). Todavía queremos servir ambos platos al mismo tiempo. Aquí está un ejemplo que ilustra cómo utilizar los métodos $q.all y $q.when para archivar esto de una manera muy elegante:


it('should illustrate promise aggregation with $q.when', function () {

	var ordersDelivered = $q.all([
		pizzaPit.takeOrder('Pepperoni'),
		$q.when('home made salad')
	]);
	ordersDelivered.then(pawel.eat, pawel.beHungry);

	pizzaPit.deliverOrder();

	expect($log.info.logs).toContain(['Pawel is eating delicious Pepperoni, home made salad']);

});

El metodo $q.when retorna una promesa que es resuelta con un valor proporcionado como un argumento a la llamada del método when.


Integración del servicio $q en AngularJS


El servicio no solo es una muy capaz (pero ligera!) implementación de la API Promise, sino que también está estrechamente integrada con la maquinaria de representación de AngularJS.

En primer lugar, las promesas pueden ser directamente expuestas en un ámbito y automáticamente representadas tan pronto como una promesa es resuelta. Esto nos permite tratar las promesas como valores del modelo. Por ejemplo, teniendo en cuenta la siguiente plantilla:

<h1>Hello, {{name}}!</h1>

Y el código en un controlador:

$scope.name = $timeout(function () {
	return "World";
}, 2000);

El famoso texto "Hello, World!" será representado luego de dos segundos sin ninguna intervención manual de los programadores.

NOTA: El servicio $timeout retorna una promesa que será resuelta con un valor retornado desde una retrollamada de tiempo de espera.

Tan conveniente como podría ser este patrón resulta en un código que no es muy legible. Las cosas pueden ser aún más confusa cuando nos damos cuenta de que las promesas retornadas de una llamada de función no se representan de forma automática! El marcado de la plantilla es el siguiente:

<h1>Hello, {{getName()}}!</h1>

Y el siguiente código en un controlador:

$scope.getName = function () {
return $timeout(function () {
return "World";
}, 2000);
};

Este código no producirá el texto esperado en una plantilla.


TIP: No se aconseja exponer promesas directamente sobre un $scope y contar con la representación automática de los valores resueltos. Nosotros encontramos con este enfoque es bastante confuso, sobre todo teniendo en cuenta el comportamiento incoherente de promesas retornadas desde una función llamada.

La API promise con $http

Ahora que hemos cubierto las promesas podemos desmitificar el objeto respuesta siendo retornado desde la llamada al método $http. Si recuerdan un simple ejemplo desde el principio de éste capítulo usted recordará que las llamadas al servicio $http retornará un objeto  en la que las retrollamadas error y success pueden ser registradas. En realidad el objeto retornado es una promesa de pleno derecho con dos adicionales, convenientes métodos: success y error. Como cualquiera promesa, la retornada desde una llamada a $http tiene también el entonces método que nos permite reescribir el código de registro de retrollamadas en la siguiente forma:

var responsePromise = $http.get('data.json');

responsePromise.then(
	function (response) {
	
		$scope.data = response.data;
	
	},
	function (response) {
	
		throw new Error('Something went wrong...');
	}
);

La promesa retornada desde el servicio $http es resolvida con el objeto respuesta que tiene la siguientes propiedades: data, status, headers y config.

NOTA: Las llamadas a los métodos de servicio $http retornaran promesas con dos métodos adicionales (success y error) para registrar retrollamadas facilmente.  


Ya que el servicio $http retorna promesas de las llamadas de sus métodos nosotros podemos disfrutar del todo el poder de la API Promise mientras interactuamos con un back-end. Nosotros podemos fácilmente agregar retrollamadas, encadenar y unir solicitudes como también tomar ventaja del manejo de errores sofisticado en el mundo asíncrono.

Comunicación con los puntos finales del servicio REST

La transferencia de estado representacional (REST) es una opción de arquitectura popular para exponer servicios a través de una red. La interfaz proporcionada por $http nos permite interactuar fácilmente con los puntos finales del servicio REST desde cualquier aplicación web basada en AngularJS. Pero AngularJS da un paso más, y proporciona un servicio dedicado $resource para hacer interacciones con los puntos finales del servicio REST  aún más fáciles.
El servicio $resource

Los puntos finales del servicio REST a menudo exponen operaciones CRUD que son accesibles llamando diferentes métodos HTTP en un conjunto de URLs similares. El código que interactúa con tales puntos finales es usualmente sencillo pero tedioso de escribir. El servicio $resource nos permite eliminar el código repetitivo. Podemos también empezar a operar a un  nivel de abstracción alto y pensar en la manipulación de datos en términos de objetos (recursos) y llamar métodos en lugar hacer llamadas HTTP de bajo nivel.

NOTA: El servicio $resource es distribuido en un archivo separado (angular-resource.js), y reside en un módulo dedicado (ngResource). Para tomar ventaja del servicio $resource nosotros necesitamos incluir el archivo angular-resource.js y declarar la dependencia en el módulo ngResource desde el módulo de nuestra aplicación.

Para ver lo fácil que es interactuar con los puntos finales del servicio REST usando el servicio $resource nosotros vamos a construir una abstracción sobre el conjunto de usuarios expuestos como un servicio RESTful por el MongoLab.









angular.module('resource', ['ngResource'])

	.factory('Users', function ($resource) {
		return  $resource('https://api.mongolab.com/api/1/databases/ascrum/collections/users/:id',
				{				
					apiKey:'4fb51e55e4b02e56a67b0b66',
					id:'@_id.$oid'
				}
			);
	})

Nosotros empezamos registrando una receta (una fábrica) para la función constructora Users. pero nota que nosotros no necesitamos escribir ningun codigo para esta función constructora. Es el servicio $resource quien preparara la implementación por nosotros.

El servicio $resource generara un conjunto de métodos que hacen que sea fácil de interactuar con los puntos finales del servicio REST. Por ejemplo, consultando por todos los usuarios en el almacén de persistencia es tan simple como escribir:

.controller('ResourceCtrl', function($scope, Users){
$scope.users = Users.query();
});

¿Qué sucederá en la llamada al método User.query()? el código generado $resource va a preparar y emitir una llamada $http. Cuando una respuesta esta lista, la cadena entrante JSON será convertida a un array JavaScript donde cada elemento de este array es de tipo Users.   

NOTA: Las llamadas al servicio $resource retornaran una función constructora argumentada con métodos para interactuar con un punto final del servicio REST:  query, get, save y delete.

AngularJS requiere poca información para generar un recurso funcional completo. Vamos a examinar los parámetros para el método $resource para ver que entrada es requerida y que puede ser personalizado:

$resource('https://api.mongolab.com/api/1/databases/ascrum/collections/users/:id', {
	 apiKey:'4fb51e55e4b02e56a67b0b66',
	 id:'@_id.$oid'
});


El primer argumento es una URL o más bien un patrón URL. El patron URL puede contener  espacios reservados con nombre que comienzan con el carácter de dos puntos. Podemos especificar sólo un patrón URL lo que significa que todos los verbos HTTP deben utilizar URLs muy similares.

TIP:  Si su back-end usa un numero de puerto como parte de la URL, el numero de puerto necesita ser escapado mientras suministramos el patrón de la URL a la llamada del servicio $resource (por ejemplo, http://example.com\\:3000/api). Este es requerido ya que los “dos puntos” tienen un significado especial en el patrón de la URL de $resource’s.

El segundo argumento de la función $resource nos permite definir parámetros por defectos que deberían ser enviados con cada solicitud. Tenga en cuenta que aquí por "parámetros" significan que ambos marcadores de posición en una plantilla de URL, y los parámetros de solicitud estándar enviados como una cadena de consulta. AngularJS intentarán primero de "rellenar agujeros" en la plantilla de URL, y luego añadirán los  parámetros restantes a la cadena de consulta de la URL.

Los parámetros por defecto pueden ser cualquiera de los dos estáticos (especificado en una fábrica) o dinámicos, tomado de un objeto de recurso. Los valores de parámetros dinámicos van precedidos de un carácter @.      

Métodos a nivel de la instancia y a nivel del constructor

El servicio $resource automáticamente genera dos conjuntos de métodos convenientes. Un conjunto de métodos serán generados en el nivel del constructor (a nivel de la clase) para un recurso dado. El objetivo de estos métodos es operar en colecciones de recursos o atender la situación en la que no tenemos ninguna instancia de recurso creado. El otro conjunto de métodos estará disponible en una instancia de un recurso en particular. Esos métodos a nivel de instancia son responsables de interactuar con un recurso (un registro en un almacén de datos).







Métodos a nivel del constructor

La función constructora generada por el servicio $resource tiene un conjunto de métodos correspondientes a diferentes verbos HTTP:

Users.query(params, successcb, errorcb): Se emite una solicitud HTTP GET y espera un array en la respuesta JSON. Se utiliza para recuperar una colección de artículos.

Users.get(params, successcb, errorcb): Se emite una solicitud HTTP GET y espera un objeto en la respuesta JSON. Se utiliza para recuperar un solo artículo.

Users.save(params, payloadData, successcb, errorcb): Se emite una solicitud HTTP POST con el cuerpo de la solicitud generada a partir de la carga útil.

Users.delete(params,successcb, errorcb) (y sus alias: Users.remove): Se emite una solicitud  HTTP DELETE.

Para todos los métodos listados anteriormente: successcb y errorcb denotan unas funciones de retrollamada success y error, respectivamente. Los argumentados parámetros (params) nos permiten especificar parámetros por acción que se va a terminar, ya sea como parte de la URL o como un parámetro en una cadena de consulta. Por último, la argumentada carga útil de datos (payloadData) nos permite especificar el cuerpo de la petición HTTP cuando proceda (solicitudes PUT y POST).

Métodos a nivel de la instancia


El servicio $resource no solo generara una función constructora, sino que además agrega métodos a nivel del prototipo (instancia). Lo métodos a nivel de la instancia son equivalentes a sus contrapartes a nivel de la clase, pero operar de una sola instancia. Por ejemplo, un usuario puede borrar ya sea llamando a:

Users.delete({}, user);

O mediante la invocación de un método de la instancia del usuario como:

user.$delete();


Los métodos a nivel de la instancia son muy convenientes, y nos permite escribir código conciso para manipular recursos. Vamos a ver otro ejemplo para guardar un nuevo usuario:

var user = new Users({
name:'Superhero'
});

user.$save();


Esto puede ser reescrito usando el método save que esta a nivel de la clase:

var user = {
name:'Superhero'
};

Users.save(user);

TIP: La fábrica $resource genera ambos métodos a nivel de la instancia y a nivel de la clase. Los métodos a nivel de la instancia llevan el prefijo con el carácter $. Ambos métodos tienen funcionalidad equivalente por lo que depende de usted elegir la forma más conveniente en función de su uso-caso.

Métodos personalizado

Por defecto la fábrica $resource genera un conjunto de métodos que es suficiente para los típicos usos de caso. Si un back-end usa diferentes verbos HTTP para ciertas operaciones (por ejemplo PUT o PATCH) es relativamente sencillo añadir métodos personalizados en un nivel de recursos.

TIP: Por defecto la fábrica $resource no genera ningún método correspondiente a solicitudes HTTP PUT. Si su back-end mapea cualquier operación de solicitudes HTTP PUT usted deberá añadir esos métodos manualmente.

Por ejemplo, la API REST de MongoLab esta usando el método POST HTTP para crear nuevos artículos pero el método PUT debe ser usado para actualizar entradas existentes. Vamos a ver como definir un personalizado método para actualizar (ambos update a nivel de la clase y $update a nivel de la instancia):






.factory('Users', function ($resource) {
	 return $resource(
		'https://api.mongolab.com/api/1/databases/ascrum/collections/users/:id',
		{
			apiKey:'4fb51e55e4b02e56a67b0b66',
			id:'@_id.$oid'
		},
		{
			update: {method:'PUT'}
		}
	 );
})


Como podrás ver definir un nuevo método es tan simple como suministrar un tercer parámetro a la función factory $resource. El parámetro debe ser un objeto con la siguiente forma:

action: {method:?, params:?, isArray:?, headers:?}

La llave action es un nuevo nombre del método a ser generado.  Un método generado emitirá una solicitud HTTP especificada por (method, params) sosteniendo los parámetros por defecto específica de esta acción en particular y el específico isArray,  si la data retornada desde un back-end representa una colección (un array) o un objeto individual. También es posible especificar cabeceras HTTP personalizadas.

El servicio $resource puede solo trabajar con arrays JavaScript y objetos como data recibida desde un  back-end. Valores individuales (tipos primitivos) no son soportados.
Los métodos que retorna una colección (aquellos marcados con isArray) deben retornar un array JavaScript. Los arrays envueltos dentro de un objeto JavaScript no serán procesados como se esperaba.



Añadiendo comportamiento a los objetos de recursos.

La fábrica $resource genera funciones constructoras que pueden ser usadas como cualquier otro constructor JavaScript para crear nuevas instancias de recursos usando la palabra clave new. Pero nosotros podemos también extender el prototipo de este constructor para añadir un nuevo comportamiento de los objetos de recursos. Digamos que nosotros queremos tener un nuevo método en el nivel de usuario la salida de un nombre completo basado en un nombre y apellido. Esta es la manera recomendada de conseguir esto:


.factory('Users', function ($resource) {

	var Users = $resource(
		'https://api.mongolab.com/api/1/databases/ascrum/collections/users/:id',
		{
			apiKey:'4fb51e55e4b02e56a67b0b66',
			id:'@_id.$oid'
		},
		{
			update: {method:'PUT'}
		}
	);

	Users.prototype.getFullName = function() {
		return this.firstName + ' ' + this.lastName;
	};
	
	return Users;

})


Añadiendo nuevos métodos a nivel de la clase (constructora) es también posible. Ya que, en  JavaScript una función es un objeto de primera clase, podemos definir nuevos métodos en una función constructora. De esta forma podemos añadir métodos personalizados “a mano” en lugar de confiar en la generación de metodos automatica de AngularJS. Esto puede resultar útil, Si necesitamos alguna lógica no estándar en uno de los métodos de recursos. Por ejemplo, la API REST MongoLab requiere que el identificador de un objeto es removido de la carga útil mientras emiten una solicitud PUT (actualizar).    

$resource crea métodos asíncronos

Tengamos una segunda mirada en el método de consulta de ejemplo:

$scope.users = Users.query();

Podríamos tener la impresión que los recursos generados se comportan de la manera síncrona (no estamos usando las funciones de retrollamadas o promesas aquí). En realidad, la llamada al método de consulta es asíncrona y AngularJS utiliza un truco inteligente para hacer que parece síncrona. AngularJS simplemente mantendrá una referencia a una matriz devuelta al principio,

Lo que está pasando aquí es que AngularJS retornará inmediatamente de una llamada al método Users.query() un array vacío como resultado. Entonces, cuando la llamada asincrónica es exitosa, y los datos reales llega desde el servidor, el array se actualizará con los datos. AngularJS simplemente mantiene una referencia a un array devuelto al principio, y lo llenará cuando los datos estén disponibles. Este truco funciona en AngularJS ya que al llegar la data el contenido del array retornado cambiará y las plantillas conseguirán ser actualizadas automáticamente.

Pero no se confunda, las llamadas a $resource son asíncronas. Esto es a menudo una fuente de confusión, ya que es posible que desee para tratar de escribir el siguiente código (o acceder a la matriz inicial de cualquier otro modo):

	$scope.users = Users.query();
console.log($scope.users.length);

Y no funcione como se esperaba!

Afortunadamente es posible usar retrollamadas en el método generado por la fábrica $resource y reescribir el código precedente para hacer que se comporte como es debido:

Users.query(function(users){
$scope.users = users;
	console.log($scope.users.length);
});


NOTA: Los métodos generados por la fábrica son asíncronos, aunque AngularJS esté usando un truco inteligente, y la sintaxis pudiera sugerir que nosotros estamos tratando con métodos síncronos.

Limitaciones del servicio $resource

La fábrica factory es un servicio útil, y nos permite empezar a hablar a back-ends RESTful en prácticamente poco tiempo. Pero el problema con $resource es que es un servicio genérico; no adaptado a las necesidades de back-end particulares, Como tal, toma algunos supuestos que podrían no ser cierto para el back-end de nuestra elección.  

Si la fábrica $resource funciona para su back-end y aplicación web, eso es genial. Hay muchos casos de uso donde $resource podría ser suficiente, pero para las aplicaciones sofisticadas a menudo es mejor utilizar el servicio de $http de nivel inferior.

Adaptadores REST personalizados con $http

La fábrica $resource es muy útil, pero si usted toca sus limitaciones es relativamente facil de crear un fabrica tal como $resource personalizada basada en el servicio $http. Al tomar tiempo de escribir una fábrica de recurso personalizada podemos ganar total control sobre las URLs y procesamiento pre/post de la data. Como beneficio adicional ya no tendrá que incluir el archivo agular-resource.js y así ahorrar unos cuantos KB del peso total de la página.

Lo que sigue es un ejemplo simplificado de una fábrica tal como resource personalizada dedicada a la API RESTful de MongoLab. Familiarizarse con la API Promise es la llave para entender esta implementación.

angular.module('mongolabResource', [])
	
	.factory('mongolabResource', function ($http, MONGOLAB_CONFIG) {
		 return function (collectionName) {
			
			//basic configuration
			var collectionUrl = 'https://api.mongolab.com/api/1/databases/' + MONGOLAB_CONFIG.DB_NAME + '/collections/' + collectionName;
			var defaultParams = {apiKey:MONGOLAB_CONFIG.API_KEY};
			
			//utility methods
			var getId = function (data) {
				return data._id.$oid;
			};
			
			//a constructor for new resources
			var Resource = function (data) {
				angular.extend(this, data);
			};
			
			Resource.query = function (params) {
				
				return $http.get(collectionUrl, {
					params:angular.extend(
						{q:JSON.stringify(params)},
						defaultParams
					)
				}).then(function (response) {
	
					 var result = [];
					
					 angular.forEach(response.data, function (value, key) {
						result[key] = new Resource(value);
					 });
					
					 return result;

				});
			};
			
			Resource.save = function (data) {
				return $http.post(collectionUrl, data, {params:defaultParams})
					.then(function (response) {
							return new Resource(data);
					});
			};
			
			Resource.prototype.$save = function (data) {
				return Resource.save(this);
			};

			Resource.remove = function (data) {
			 return $http.delete(collectionUrl + '', defaultParams)
				 .then(function (response) {
					return new Resource(data);
				 });
			};
			
			Resource.prototype.$remove = function (data) {
				return Resource.remove(this);
			};
			
			//other CRUD methods go here
			//convenience methods
			Resource.prototype.$id = function () {
				return getId(this);
			};
			
			return Resource;
		
		 };
	});


El código de ejemplo comienza declarando un nuevo módulo (mongolabResource) y la fábrica (mongolabResource) aceptan un objeto de configuración (MONGOLAB_CONFIG). Esas son las partes que debería parecer familiares por ahora. Basado en un proporcionado objeto de configuración se puede preparar una URL para ser utilizada por un recurso. Aquí tenemos el control total de cómo se crea una dirección URL y manipulada.

Entonces, el constructor Resource es declarado, para que podamos crear nuevos objetos de recursos desde los datos existentes. Lo que sigue es una definición de varios método: query, save y remove. Estos métodos son definidos a nivel del la clase constructora, pero los métodos a nivel de la clase también se declaran (cuando proceda) para seguir la misma convención como la original implementación $resource. Proporcionar a nivel de la instancia de métodos es tan fácil como delegar a la nivel de la clase:

Resource.prototype.$save = function (data) {
return Resource.save(this);
};

Uso de encadenamiento de promesas es la parte crucial de la implementación de recursos personalizados. El método then es siempre llamado en una promesa retornada desde una llamada $http, y una promesa resultantes es retornada a el cliente. Una retrollamada  success en una promesa retornada desde una llamada $http es usada para registrar lógica post procesamiento. Por ejemplo en la implementación del método query nosotros post procesamos raw JSON retornada desde un back-end, y creamos instancias de recursos. Aqui, una vez más hemos conseguido total control sobre el proceso de extracción de datos desde una respuesta.

Vamos a examinar como una fábrica de recursos puede ser usada:


angular.module('customResourceDemo', ['mongolabResource'])
	
	.constant('MONGOLAB_CONFIG', {
		 DB_NAME: 'ascrum',
		 API_KEY: '4fb51e55e4b02e56a67b0b66'
	})
	
	.factory('Users', function (mongolabResource) {
		return mongolabResource('users');
	})
	
	.controller('CustomResourceCtrl', function ($scope, Users, Projects) {
		
		Users.query().then(function(users){
				$scope.users = users;
			});
			
	});



El uso de nuestros recursos personalizados no difiere mucho del original equivalente $resource. Para empezar tenemos que declarar una dependencia en el módulo donde una fabrica de recursos personalizados es definida (mongolabResource). A continuación, tenemos que proporcionar la opciones de configuración requerida en forma de una constante. Tan pronto como la configuración inicial esta lista podemos definir los recursos reales creados por una fábrica personalizada. Es tan fácil como invocar la función fabricante mongolabResource y pasar un nombre de colección MongoDB como argumento.

Durante el tiempo de ejecución de la aplicaciones un nuevo constructor de recursos es definido (aqui: Users) puede ser inyectado como cualquier otra dependencia. Entonces un constructor inyectado puede ser usado para invocar cualquiera método a nivel de la instancia o a nivel de la clase. Un ejemplo:    

$scope.addSuperhero = function () {
new Users({name: 'Superhero'}).$save();
};

La mejor parte de cambiarse a una versión personalizada basada en $http de una fábrica es que nosotros podemos disfrutar de todo el poder la API promise.

Usando características avanzadas de $http

El servicio $http es extremadamente flexible y poderoso. Sus superpoderes son resultado de la limpia, flexible API y el uso de la API promesa. En esta sección, nosotros vamos a ver cómo podemos usar algunas de las características más avanzadas del servicio $http.
Interceptando respuestas

El servicio $http en AngularJS nos permite registrar interceptores que serán ejecutadas alrededor de cada solicitud. Tales interceptores son muy útiles en situaciones donde nosotros hacer procesamiento especial para muchas (potencialmente todas) solicitudes.

Como el ejemplo inicial, vamos a asumir que nosotros queremos reintentar solicitudes fallidas. Para ello podemos definir un interceptor que inspecciona el código de estatus de la respuesta y reintenta la solicitud, si el código de estado (503) Servicio HTTP No disponible es detectado. El boceto de el código podría parecer:

angular.module('httpInterceptors', [])
.config(function($httpProvider){
$httpProvider.responseInterceptors.push('retryInterceptor');
})
.factory('retryInterceptor', function ($injector, $q) {
return function(responsePromise) {
return responsePromise.then(null, function(errResponse) {
if (errResponse.status === 503) {
return $injector.get('$http')(errResponse.config);
} else {
return $q.reject(errResponse);
}
});
};
});

Un interceptor es una función que acepta una promesa de la solicitud original como su argumento, y debe retornar otra promesa acordando interceptar un resultado. Aquí donde inspeccionamos el código errResponse.status para verificar si nosotros estamos en la condición de error donde podamos tratar de recuperar. Si es así, entonces estamos retornado un promesa desde una llamada $http completamente nueva hecha con el mismo objeto de configuración como la solicitud original. Si llegamos interceptar un error que no podamos manejar nosotros simplemente propagamos este error (metodo $q.reject).

Los interceptores AngularJS hacer que tenga que usar la API promesa, y esto es lo que los hace tan poderoso. En el ejemplo descrito aquí podemos reintentar una solicitud HTTP en una forma completamente transparente a los clientes que usen el servicio $http. El Capítulo 7 Asegurando Su Aplicación, tiene una ejemplo completo de la utilización de los interceptores de respuesta para proporcionar mecanismo de seguridad sofisticados.     

Registrar nuevos interceptores es sencillo, y se reduce a la adición de una referencia a un nuevo interceptor (en este caso un servicio AngularJS creado a través de la fábrica) a un array interceptor mantenidos por los $httpProvider. Tenga en cuenta que estamos usando un proveedor para registrar nuevos interceptores y los proveedores sólo están disponibles en bloques de configuración.
Probando el código que interactúa con $http

Probar el código llamando servicios HTTP externos es a menudo problemático debido a la data cambiante y a la latencia de la red. Queremos que nuestras pruebas corran rápido  y sean predecibles. Afortunadamente, AngularJS provee excelentes objetos ficticios para simular respuestas HTTP.

En AngularJS el servicio $http depende de otro servicio de bajo nivel $httpBackend. Podemos pensar en el servicio $httpBackend como una cosa que envuelve el objeto XMLHttpRequest. Esta envoltura cubre las incompatibilidades de los navegadores y permite solicitudes JSONP.

NOTA: El código de la aplicación nunca debería llamar el servicio $httpBackend directamente, como el servicio $http provee una abstracción mucho mejor. Pero teniendo un servicio $httpBackend separado significa que nosotros podemos intercambiarlo por una imitación durante las pruebas.

Para ver la imitación de $httpBackend aplicadas a las pruebas unitarias nosotros vamos a examinar una prueba para un controlador de ejemplo desencadenando una solicitud GET vía el servicio $http. Aqui esta el codigo para el controlador mismo:





.controller('UsersCtrl', function ($scope, $http) {
 $scope.queryUsers = function () {
   $http.get('http://localhost:3000/databases/ascrum/collections/
   users')
     .success(function (data, status, headers, config) {
       $scope.users = data;
     }).error(function (data, status, headers, config) {
       throw new Error('Something went wrong...');
     });
 };
});

Para probar el código de este controlador se puede escribir:

describe('$http basic', function () {
 var $http, $httpBackend, $scope, ctrl;
 beforeEach(module('test-with-http-backend'));
 beforeEach(inject(function (_$http_, _$httpBackend_) {
   $http = _$http_;
   $httpBackend = _$httpBackend_;
 }));
 beforeEach(inject(function (_$rootScope_, _$controller_) {
   $scope = _$rootScope_.$new();
   ctrl = _$controller_('UsersCtrl', {
     $scope : $scope
   });
 }));
 it('should return all users', function () {
//setup expected requests and responses
   $httpBackend
     .whenGET('http://localhost:3000/databases/ascrum/collections/users').
     respond([{name: 'Pawel'}, {name: 'Peter'}]);
//invoke code under test
   $scope.queryUsers();
//simulate response
   $httpBackend.flush();
//verify results
   expect($scope.users.length).toEqual(2);
 });
 afterEach(function() {
   $httpBackend.verifyNoOutstandingExpectation();
   $httpBackend.verifyNoOutstandingRequest();
 });
});
Primero que todo podemos ver que la imitación de $httpBackend nos permite especificar solicitudes esperadas ( whenGET(...) ) y preparar respuestas falsas
( respond(...) ). Hay toda una familia de métodos whenXXX para cada vervo HTTP. La firma de la familia de métodos whenXXX es bastante flexible, y nos permite especificar URLs como expresiones regulares.

Para tomar la data normalmente retornada desde un back-end nostros podemos usar el método respond. Es posible imitar las cabeceras de respuesta también.      

          La mejor parte sobre la imitación de $httpBackend es que nos permite obtener control preciso sobre las respuestas y sus tiempos. Usando el método flush() no estamos más a la merced de los eventos asíncronos, incluso si el servicio $http es asíncrono por diseño. Esto hace la ejecucion pruebas unitarias rápidas y de forma previsible.     

El metodo verifyNoOutstandingExpectation verifica que se hicieron todas las llamadas esperadas (los métodos $http invocados y las respuestas completadas), mientras que la llamada verifyNoOutstandingRequest se asegura que el código bajo prueba no desencadene cualquier inesperada llamada XHR. Usando aquellos dos métodos nosotros podemos asegurarnos de que el código bajo prueba invoca todos los métodos esperados y sólo los esperados.



















Resumen

Este capítulo nos guío a través de diferentes métodos de comunicación con el
back-end para recibir y manipular data. Empezamos nuestro viaje observando la APIs $http lo cual es el servicio fundamental para emitir solicitudes JSONP y XHR en AngularJS. No solo nos familiarizados con lo básico de las APIs de el servicio $http, pero también tuvimos un vistazo de cerca a las diferentes formas de lidiar con solicitudes de origen cruzado (cross-origin requests).

	Muchos de los servicios Asíncronos AngularJS confían en la API Promesa para proveer interfaces elegantes. El servicio $http depende fuertemente en promesas así que tuvimos que cubrir exhaustivamente la implementación de promesas en AngularJS. Vimos que el servicio $q provee una general propuesta de la API Promesa, y está estrechamente integrado con el mecanismo de representación. Teniendo un buen entendimiento de la API Promesa con el servicio $q nos permite entender completamente como los valores son retomados desde las llamadas al método $http.



AngularJS puede fácilmente comunicarse con puntos finales  RESTful. Existe la dedicada fabrica $resource que hace muy fácil escribir código interactuando con back-ends RESTful. La fábrica $resource es muy conveniente pero es muy genérica, y como tal puede no cubrir todas sus necesidades. No debemos rehuir de crear un personalizada fábrica tal como $resource basada en la API $http.

Hacia el final del capítulo, hemos cubierto algunos de los usos avanzados de la API $http con interceptores de respuesta.

Por último, como cualquier código JavaScript, los métodos que usan la API $http deberían ser probadas a fondo, y vimos que AngularJS provee excelentes objetos ficticios para facilitar las pruebas unitarias de código interactuando con un back-end.

Con todos los datos cargados en el lado del cliente, y disponibles en JavaScript estamos listos para sumergirnos en varias formas de representar esa data con AngularJS. El proximo capitulo esta completamente dedicado a las plantillas, directivas y representación.     
 



















