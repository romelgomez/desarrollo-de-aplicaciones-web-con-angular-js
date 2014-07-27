## Capítulo 7. Asegurar su aplicación

En cualquier aplicación web, debemos asegurarnos que los datos y acciones sensibles no están disponibles a usuarios no autorizado. El único lugar seguro en tal aplicación es dentro del servidor. Fuera de este, debemos asumir que el código puede estar comprometido, por lo que debemos colocar controles en el lugar, en el punto donde los datos entran o salen de nuestro servidor. La primera parte de este capítulo examina lo que debemos hacer en ambos lados, cliente y servidor para garantizar esta seguridad, como se indican en los siguientes puntos:

- Asegurar el servidor para prevenir el acceso no autorizado a los datos y al HTML.       
- Encriptar la conexión para prevenir fisgones.
- Prevenir ataques de secuencias de comandos entre sitios (**cross-site scripting, XSS**), y solicitudes falsificadas entre sitios (**cross-site request forgery, XSRF**).
- Bloqueando una vulnerabilidad, inyección de JSON.  

Si bien los controles de seguridad siempre deben ser ejecutados en el servidor, y esto es lo mas critico. debemos también proporcionar una buena experiencia de usuario con una interfaz de cliente que solo de acceso al usuario a funcionalidades apropiadas a sus permisos. Debemos también proporcionar un proceso de autenticación limpio que no altere el flujo de su interacción con la aplicación. La segunda parte de este capítulo examina la mejor forma de soportar esto en AngularJS, como se da en los siguientes puntos:

- La diferencia en asegurar una aplicación cliente enriquecida con estado comparada con las tradicionales aplicaciones basada en el servidor sin estado.  
- Manejo de errores de autorización del servidor interceptando respuestas HTTP.   
- Restringir el acceso a partes de la aplicación asegurando rutas  
 
## Proporcionado autenticacion y autorizacion del lado del servidor

Una cosa es común para todas las aplicaciones cliente/servidor, es que el servidor es el único lugar donde los datos están seguros. No podemos confiar en el código del lado del cliente para bloquear el acceso a información sensible. Similarmente, el servidor nunca debe confiar en el cliente para validar datos que este envía.

> `NOTA` Esto es particularmente relevante en aplicaciones JavaScript, donde es muy sencillo leer el código fuente, y a continuación, incluso modificarlo para que ejecute acciones maliciosas.         

En la real aplicación web, nuestro servidor debe proporcionar el nivel apropiado de seguridad. Para nuestra aplicación de ejemplo, el servidor tiene muy simples medidas de seguridad en el lugar. Implementamos autenticacion y autorizacion usando Passport, el cual es un plugin ExpressJS, El ID del usuario autenticado es almacenado como parte de una sesión cookie encriptada. Esta se transmite al navegador, cuando el usuario inicie sesión. Esta cookie es enviada al servidor en cada solicitud, para permitir al servidor autenticar la solicitud.      

### Manejando accesos no autorizados

Cuando una solicitud es realizada a una URL, el servidor determina si esta URL requiere un usuario autenticado, y si el usuario tiene suficientes privilegios, hemos arreglado que el servidor responda con un código de error HTTP 401 No autorizado, si el cliente intenta realizar cualquiera de los siguiente:   

- Cualquiera solicitud no GET (por ejemplo POST, PUT, o DELETE) en una colección de la base de datos, cuando no hay un usuario autenticado.   
- Cualquiera solicitud no GET en la colecciones de la base de datos de usuarios o proyectos.

De esta manera estamos en condiciones de asegurar los datos (solicitudes JSON) de accesos no autorizados. Podemos hacer lo mismo con otros elementos tales como HTML o imágenes, si queremos controlar el acceso a esos también.    

### Proporcionado una API de autenticación del lado del servidor

Para permitir que la aplicación cliente autentifique los usuarios, nuestro servidor expone la siguiente API HTTP:

- POST /login : Este mensaje auténtica al el usuario pasando los parámetros username y password, pasados en el cuerpo de la solicitud POST.
- POST /logout : Este mensaje cierra la sesión del usuario actual removiendo la cookie de autenticación.
- GET /current-user: Este mensaje recupera la información del usuario actual.

Esta interfaz es suficiente para nosotros demostrar cómo manejar la autenticación y autorización en nuestra aplicación.

> `NOTA` En una aplicación comercial, los requerimientos de seguridad deben ser más complejos, y tal vez también prefieras usar un esquema de autenticación de terceros tal como OAuth2 (see http://oauth.net/).

## Asegurando plantilla parciales

Hay situaciones donde no querrás que los usuarios tengan acceso a las plantillas parciales (HTML) para las rutas AngularJS a las que ellos no tienen autorización. Tal vez, las plantillas contienen información de diseño que implícitamente expone información sensitiva.
 
En este caso, una solución simple es asegurar que las solicitudes para estas plantillas parciales comprueben su autorización en el servidor. En primer lugar, no debemos precargar tales plantillas al inicio de la aplicación. Y entonces, debemos configurar el servidor para que compruebe el actual usuario siempre que una de estas plantillas parciales es solicitada, una vez más retornado un código de error HTTP 401 No autorizado, si no esta autorizado.

> `NOTA` Si estamos confiando en el servidor para comprobar la autorización en cada solicitud de parcial, necesitamos asegurarnos que el navegador (o cualquier proxy) no esté almacenando en caché las solicitudes para los parciales. Para hacer esto, el el servidor debe proporcionar las siguientes cabeceras HTTP, al servir estos parciales:

![chapter-7-1](/img/chapter-7-1.png)

AngularJS almacena en caché todas las plantillas que descarga en el servicio `$templateCache`. Si queremos asegurar nuestros parciales, debemos también asegurar que los parciales no son almacenados en la caché de una manera que permita no autorizar el acceso.  Debemos borrar esas plantillas del servicio `$templateCache` antes de que un nuevo usuario entre, para asegurar que el nuevo usuario inadvertidamente tenga acceso a las plantillas.  

Podemos borrar plantillas del servicio `$templateCache`, de cualquiera de las dos, selectivamente o incluso completamente en varios puntos de la aplicación. Por ejemplo, podríamos despejar las plantillas restringidas cuando se navega fuera de una ruta restringida, o cuando cierra la sección. Esto puede ser complicado de manejar, y el método más seguro es quizás recargar completamente la página, es decir refrescar el navegador cuando el usuario cierre la sesión.  

> `TIP` Recargar la aplicación, mediante la actualización de la página del navegador, tiene el beneficio añadido de despejar cualquier dato que pudo haber sido almacenado en la caché en los servicios AngularJS.

## Detener ataques maliciosos

Para ser capaz de permitir un acceso seguro a usuarios legítimos, tiene que haber un elemento de confianza entre el servidor y el navegador. Desafortunadamente, hay una serie de ataques que se pueden aprovechar de esta confianza. Con el correcto soporte en el servidor, AngularJS puede proporcionar protección contra estos agujeros de seguridad.

### Prevenir el espionaje de cookie (ataques de hombre-en-el-medio)

Siempre que pases datos sobre HTTP entre un cliente y un servidor, hay una oportunidad para que terceros espien en la información segura, o incluso peor, acceder a sus cookies para secuestrar su sección y acceder al servidor, como si fuera usted.  Esto se denomina a menudo un ataque “hombre-en-el-medio”, consulte: http://es.wikipedia.org/wiki/Ataque_Man-in-the-middle. La forma más fácil de prevenir estos ataques es usar HTTPS en lugar de HTTP.

> `NOTA` En cualquier aplicación, en la cual los datos sensibles pasan entre la aplicación y el servidor debe usar HTTPS para asegurar que los datos están encriptados.  

Encriptando la conexión usando HTTPS, prevenimos que los datos sensibles sean leídos a medida que pasan entre el cliente y el servidor, y además prevenimos que los usuarios no autorizados lean cookies de autenticación de nuestras solicitudes y secuestren nuestra sección.

En nuestra aplicación de ejemplo, las solicitudes a la base de datos en MongoLab ya se envian a traves de HTTPS de nuestro servidor. Para proporcionar seguridad completa de este tipo de espía, debemos además asegurar que nuestro cliente interactúe con nuestro servidor a través de HTTPS. Generalmente, este es solo un caso a conseguir, que el servidor escuche sobre HTTPS, y que el cliente haga solicitudes sobre HTTPS.

Implementar esto en el servidor depende de su elección de la tecnología en el back-end, y esta fuera del ámbito de este libro. Pero en Node.js puedes usar el módulo `https` como se muestra en el siguiente código:   

```
var https = require('https');
var privateKey =
 fs.readFileSync('cert/privatekey.pem').toString();
var certificate =
 fs.readFileSync('cert/certificate.pem').toString();
var credentials = {key: privateKey, cert: certificate};
var secureServer = https.createServer(credentials, app);
secureServer.listen(config.server.securePort);
```

En el lado del cliente, sólo tenemos que asegurar que la URL utilizada para conectar con el servidor no está codificada con el protocolo HTTP. La forma más fácil de hacerlo es no proporcionar un protocolo en absoluto en la URL.

```
angular.module('app').constant('MONGOLAB_CONFIG', {
 baseUrl: '/databases/',
 dbName: 'ascrum'
});
```
Adicionalmente, debemos también asegurar que la cookie de autenticación está restringida a sólo solicitudes HTTPS. Podemos hacer esto estableciendo la opciones `httpOnly` y secure a true, al crear la cookie en el servidor.

### Previniendo ataques de secuencia de comandos entre sitios

Un ataque entre sitios de secuencia de comandos (cross-site-scripting - XSS) es donde un atacante logra inyectar un script del lado del cliente en una página web, cuando esta es visitada por otro usuario. Esto es particularmente malo si el script inyectado realiza una solicitud a nuestro servidor, porque el servidor asumen que el usuario autenticado esta haciendo una solicitud y lo permite.

Hay una variedad de formas en que los ataques XSS pueden aparecer. Los más comunes son cuando el contenido proporcionado por el usuario es desplegado sin ser apropiadamente escapado para prevenir que HTML malicioso sea representado. La próxima  sesión explica como podemos hacer esto en el cliente, pero también debe garantizar que cualquier contenido proporcionado por el usuario es desinfectado en el servidor, antes de ser guardado o enviado de vuelta al cliente.

### Asegurar el contenido HTML en expresiones AngularJS

AngularJS escapa todo el HTML en texto, que es desplegado a través de la directiva `ng-bing`, o la plantilla de interpolación (que es texto en `{{llaves}}`). Por ejemplo, usando la siguiente modelo:

```
$scope.msg = 'Hello, <b>World</b>!';
```

Y el fragmento marcado luce de la siguiente forma:

```
<p ng-bind="msg"></p>
```

El proceso de representación escapara las etiquetas `<b>`, por lo que aparecerán como texto plano, y no como marcado:

```
<p>Hello, &lt;b&gt;World&lt;/b&gt;!</p>
```

Este enfoque proporciona una muy buena defensa contra los ataques XSS en general. Si usted actualmente quiere desplegar texto que está marcado con HTML, entonces debe o bien confiar por completo, en cuyo caso puede utilizar la directiva `ng-bind-html-unsafe`, o desinfectar el texto cargando el módulo `ngSanitize`, y a continuación, usar la directiva `ng-bind-html`.

### Permitir enlaces HTML inseguros

El siguiente en enlace representará las etiquetas `<b>` como HTML para ser interpretado por el navegador:

```
<p ng-bind-html-unsafe="msg"></p>
```

### Desinfección del HTML

AngularJS tiene una directiva más, que selectivamente desinfectara ciertas etiquetas HTML, mientras que permite que otras sean interpretadas por el navegador, la cual es `ng-bind-html`. Su uso es similar al equivalente inseguro:

```
<p ng-bind-html="msg"></p>
```

En términos de escapar, la directiva `ng-bind-html` es un término medio entre el comportamiento de la directiva `ng-bind-html-unsafe` (que permite todas las etiquetas HTML) y la directiva `ng-bind` (que no permiten etiquetas HTML en absoluto). Puede ser que sea una buena opción para casos donde queramos permitir que algunas etiquetas HTML sean ingresadas por los usuarios.

> `NOTA` La desinfección usa una lista blanca de etiquetas HTML, la cual es algo extensa. La etiquetas principales que serán desinfectadas incluyen los elementos `<style>` y `<script>`, como también atributos que toman URLs tales como `href`, `src`, y `usemap`.  

La directiva `ng-bind-html `reside en un módulo separado (`ngSanitize`), y requiere la inclusión de un archivo `angular-sanitize.js`. Debes declarar una dependencia en el módulo si planeas usar la directiva `ng-bind-html`, como se muestra en el siguiente codigo:

```
angular.module('expressionsEscaping', ['ngSanitize'])
 .controller('ExpressionsEscapingCtrl', function ($scope) {
   $scope.msg = 'Hello, <b>World</b>!';
 });
```

La directiva `ng-bind-html` usa el servicio `$sanitize`, el cual es también encontrado dentro del módulo `ngSanitize`. Este servicio es una función que toma una cadena, y entonces retorna una versión desinfectada de la cadena, como se describe en el siguiente código:

```
var safeDescription = $sanitize(description);
```

> `NOTA` A menos que estés trabajando con cualquier sistema legado (Por ejemplo CMS, back-ends enviando HTML, etc), el marcado en el modelo debe ser evitado. Tal marcado no puede contener directivas AngularJS y requiere de la directiva `ng-bind-html-unsafe` o `ng-bind-html` para obtener los resultados deseados.

### Evitar la vulnerabilidad de inyección de JSON

Hay una vulnerabilidad de inyección de JSON que permite a sitios web malignos de terceros acceder a sus recursos JSON seguros, si se retornan matrices JSON. Esto es realizado efectivamente cargando el JSON como un script en una página web, y después ejecutarlo. Consulta: http://haacked.com/archive/2008/11/20/anatomy-of-a-subtle-json-vulnerability.aspx/

El servicio `$http` tiene un solución integrada para proteger contra esto. Para prevenir que el navegador sea capaz de ejecutar el JSON retornado del recurso seguro, puede arreglar que su servidor anteponga o prefije todas las solicitudes JSON con la cadena  "`)]}',\n`", cual no es JavaScript legal, y así no puede ser ejecutado. El servicio `$http` automáticamente remueve esta cadena antepuesta, si aparece desde cualquier solicitud JSON. Por ejemplo, si su recurso JSON retorna el siguiente array:

```
['a,'b','c']
```

Este es vulnerable al ataque de inyección de JSON. En lugar de ello, el servidor debe retornar:

```
)]}',['a','b','c']
```

Esto no es JavaScript válido. Y no puede ser ejecutado por el navegador, y así no es más vulnerable al ataque. El servicio `$http` automáticamente removerá este prefijo inválido, si lo encuentra, antes de retornar el JSON válido en el objeto `response`.

### Evitar solicitudes falsificadas entre sitios

En cualquier aplicación donde el servidor debe creer quien es el usuario, es decir, que el esta logueado, y entonces permitirle el acceso a las acciones en el servidor basado en esa confianza, existe la posibilidad que otros sitios pretendan ser usted, y obtener acceso a esas mismas acciones. Esto es llamado solicitudes falsificadas entre sitios (XSRF). Si visitas un sitio malvado cuando estés logueado en un sitio seguro, el sitio web malvado puede publicar en su sitio seguro, este confía puesto que usted está autenticado actualmente.     

Este ataque es a menudo en la forma de un atributo `src` fraudulento en una etiqueta `<img>`, la cual el usuario inadvertidamente carga navegando en la pagina maligna, mientra este logueado dentro de un sitio seguro. Cuando el navegador trata de cargar la imagen, esté actualmente realiza una solicitud al sitio seguro.
 
```
<img src="http://my.securesite.com/createAdminUser?username=badguy">
```

La solución para esto es proporcionar un secreto al navegador, el cual solo puede ser accesado por JavaScript ejecutándose en el navegador, de modo que no estaría disponible en el atributo `src`. Cualquier solicitud al servidor debe incluir este secreto en sus cabeceras para probar su autenticidad.

El servicio `$http` tiene una solución integrada, con el fin de protegerse contra un ataque de esos. Usted debe hacer los arreglos para que el servidor establezca un token en la cookie de la sección, llamada XSRF-TOKEN, en la primera solicitud GET a la aplicación. Esta token debe ser único para esta sección.

En la aplicación cliente, el servicio `$http` extraerá este token de la cookie, y luego adjuntará como una cabecera (llamada X-XSRF-TOKEN) a cada solicitud HTTP que realiza. El servicio debe comprobar el token en cada solicitud, y luego bloquear el acceso si esta no es válido. El equipo AngularJS recomienda que el token sea un digest para su cookies de autenticación, con salt para mayor seguridad.      

## Añadiendo soporte de seguridad al lado del cliente

El resto de este capítulo se verá que debemos hacer en nuestras aplicaciones  ejecutándose en el navegador para proporcionar seguridad y dar una experiencia consistente al usuario, al tratar con autenticación y autorización.

Fuera de la caja, AngularJS no proporciona una funcionalidad para tratar con la autenticación y autorización. En nuestra aplicación de ejemplo, desarrollamos servicios y directivas que pueden ser usadas en nuestras plantillas y controladores para desplegar información relacionada con la seguridad, manejar los errores de autorización, y gestionar el iniciar sesión y el cierre de sesión.

![chapter-7-2](/img/chapter-7-2.png)

Este diagrama muestra dos elementos de la interfaz de usuario, el `login-toolbar` y `login-form`, ambos confían en el servicio `security`. El servicio `security` usa el servicio `$dialog` para crear un modal de diálogo desde el elemento `login-form`.

### Creando un servicio de seguridad

El servicio `security` es un componente, el cual tenemos que desarrollar proporcionado la primera API para nuestra aplicación para administrar el iniciar sesión y el cierre de sesión, y obtener información del actual usuario. Podemos inyectar este servicio dentro de los controladores y directivas. Estos pueden entonces adjuntar las siguientes propiedades y métodos del servicio en el ámbito, para tener acceso a ellos en las plantillas:

- `currentUser`: Este propiedad contiene información sobre el actual usuario autenticado, si hay alguno.  
- `getLoginReason()`: Este método retorna un mensaje localizado, explicando porque necesitamos loguearnos, por ejemplo, el actual usuario no tiene autorización.
- `showLogin()`: Este método causa que el formulario de inicio de sesión sea mostrado. Este es llamado cuando el usuario hace clic en el boton de inicio de sesión en la barra de herramientas, y cuando una respuesta de error HTTP 401 No autorizado es interceptada.
- `login(email, password)`: Este método envía los credenciales especificados al servidor para ser autenticados. Este es llamado cuando el usuario envía el formulario de inicio de sesión
- `logout(redirectTo)`: Este método cierra la sesión del actual usuario y lo redirecciona. Este es llamado cuando el usuario hace clic en el boton de cierre de sesión en la barra de herramientas.
- `cancelLogin(redirectTo)`: Este método abandona el intentar iniciar sesión,  cualquier solicitud no autorizada es descartada, y luego la aplicación redirecciona a otra `$route`. Este es llamado si el usuario cierra o cancela el formulario de inicio de sesión.

### Mostrando el formulario de inicio de sesión

Necesitamos permitir al usuario iniciar sesión. El formulario de inicio de sesión proporciona esto. Esto se compone de una plantilla (`security/login/form.tpl.html`), y un controlador (`LoginFormController`). El servicio security abre (`showLogin()`) o cierra (`login()` o `cancelLogin()`) el formulario de inicio de sesión, cuando la autenticación es requerida o solicitada.

![chapter-7-3](/img/chapter-7-3.png)

Estamos usando para esto el servicio `$dialog` del proyecto AngularUI bootstrap. Este servicio nos permite desplegar un formulario como un modal de caja de diálogo, especificando una plantilla y un controlador para el formulario.

Puedes encontrar más sobre el servicio `$dialog` en su sitio web `http://angular-ui.github.io/bootstrap/#/dialog`.

En nuestro servicio `security`, tenemos dos ayudantes: `openLoginDialog()` y `closeLoginDialog()`.

```
var loginDialog = null;
function openLoginDialog() {
 if ( !loginDialog ) {
   loginDialog = $dialog.dialog();
   loginDialog.open(
     'security/login/form.tpl.html',
     'LoginFormController')
     .then(onLoginDialogClose);
 }
}
function closeLoginDialog(success) {
 if (loginDialog) {
   loginDialog.close(success);
   loginDialog = null;
 }
}
```

Para abrir la caja de diálogo, llamamos `openLoginDialog()`, pasando la URL de la plantilla del formulario de inicio de sesión, `security/login/form.tpl.html`,  y el nombre del controlador, `LoginFormController`. Esto retorna un objeto promesa (promise) que será recibida, cuando la caja de diálogo es cerrada, el cual hacemos llamando `closeLoginDialog()`. Cuando la caja de diálogo cierra, llamamos es metodo `onLoginDialogClose()`, el cual limpiará y ejecutará el código, basado en si el usuario inició sesión o no.

No hay nada especial sobre la plantilla y el controlador, ellos solamente proporcionan un simple formulario para ingresar un correo y una clave, y conectar a el `security` y `service`. El botón Sign in (Iniciar de sesión) es manejado por `security.login()`, el botón Cancel es manejado por `security.cancelLogin()`

### Creando menús y barras de herramientas conscientes sobre la seguridad

Para una buena experiencias, no debemos mostrar cosas para las cuales el usuario no tiene permiso. Esconder elementos del usuario, sin embargo, no evitará que un determinado usuario accede a la funcionalidad, esto es puramente para prevenir que el usuario se confunda tratando de usar características para las cuales no tiene permisos. Es común hacer esta exhibición selectiva en los menú de navegación y las barras de herramientas. Para hacer más fácil el escribir plantillas que reaccionen para el actual estado de autenticación y autorización del usuario, podemos crear un servicio: `currentUser` (Usuario actual).

### Escondiendo elementos del menú

Debemos solo mostrar elementos del menú que son apropiados para los actuales permisos del usuario.

![chapter-7-4](/img/chapter-7-4.png)
   
Nuestro menú de navegación tiene un conjunto de directivas `ng-show` en los elemento que comprueban si el actual usuario tiene la autorización correspondiente, y entonces mostrar o esconder los elementos del menú en consecuencia.

```
<ul class="nav" ng-show="isAuthenticated()">
 <li ...><a href="/projects">My Projects</a></li>
 <li ... ng-show="isAdmin()">
   <a ... >Admin ...</a>
   <ul ...>
     <li><a ... >Projects</a></li>
     <li><a ... >Users</a></li>
   </ul>
 </li>
</ul>
```

Aqui puedes ver que toda el árbol de navegación está escondido si el usuario no está autenticado. Además, las opciones administrativas están escondidas, si el usuario no es un administrador.  

### Creando una barra de herramientas de sesión

Podemos crear una reusable directiva widget `login-toolbar` que simplemente despliega un botón de inicio de sesión si no hay nadie logueado, o el actual nombre del usuario y el botón de cerrar sesión, si alguien esta logueado.

![chapter-7-5](/img/chapter-7-5.png)

He aquí una plantilla para esta directiva. Inyecta los servicios `currentUser` y `security` en el ámbito de la directiva, para que podamos desplegar la información del usuario, como también mostrar o esconder los botones Log in y Log out.

```
<ul class="nav pull-right">
   <li class="divider-vertical"></li>
   <li ng-show="isAuthenticated()">
       <a href="#">{{currentUser.firstName}} {{currentUser.lastName}}</a>
   </li>
   <li ng-show="isAuthenticated()">
       <form class="navbar-form">
           <button class="btn" ng-click="logout()">Log out</button>
       </form>
   </li>
   <li ng-hide="isAuthenticated()">
       <form class="navbar-form">
           <button class="btn" ng-click="login()">Log in</button>
       </form>
   </li>
</ul>
```

## Soportando autenticación y autorización en el cliente.   

Asegurando un aplicación de cliente enriquecida, tal como la que estamos construyendo aquí con AngularJS, es significativamente diferente a asegurar una tradicional aplicación web basada en el servidor. Esto tiene un impacto en como y cuando autenticamos y autorizamos los usuarios.   

Las aplicaciones web basadas en el servidor son generalmente sin estado en el navegador. Nosotros desencadenar una solicitud de ida y vuelta para una completa nueva pagina del servidor en cada acción. Así, que el servidor puede computar los niveles de autorización en cada solicitud , y luego redireccionar a alguna página de inicio de sesión, si es necesario.

> `NOTA` En una aplicación web tradicional, basada en el servidor, podemos simplemente enviar el navegador a alguna página de inicio de sesión, y luego una vez satisfactoriamente completar el inicio de sesión, redireccionarlo a la página original de la cual solicitó el recurso seguro.       

Clientes enriquecidos no envían solicitudes de página completa en cada acción. Ellos tienden a mantener su propio estado y solo pasar datos hacia y desde el servidor. El servidor no conoce el actual estado de la aplicación, el cual hace difícil implementar la tradicional redirección a atrás, luego de ser enviado a la página de inicio de sesión. El cliente tendría que serializar el actual estado, y luego enviarlo al servidor. Luego, de satisfactoriamente iniciar la sesión, el servidor debe pasar su estado de nuevo al cliente, para que este pueda continuar donde lo dejó.

### Manejando autorizaciones fallidas.

Cuando el servidor se niega a procesar una solicitud no autorizada, retornando un error HTTP 401 no autorizado, necesitamos dar al usuario una oportunidad de autenticarse, y reintentar la solicitud. La solicitud podría haber sido parte de un proceso complejo en el cliente,  que no tiene una URL para identificarlo. Si redireccionamos a la página de inicio de sesión, entonces debemos parar la acción actual, y el usuario tendría probablemente que reanudar lo que estaban haciendo nuevamente luego de la autenticación.

En lugar de ello, vamos a interceptar las respuestas de las solicitudes no autorizadas en el servidor, antes de que lleguen de nuevo a la persona que las solicita. Vamos a suspender el actual flujo de la lógica de negocio de la aplicación, para hacer la autenticación, y entonces reintentar las solicitudes fallidas para reanudar el flujo.

![chapter-7-6](/img/chapter-7-6.png)
  
### Interceptando respuestas

Recuerda que las solicitudes `$http` retornan promesas, las cuales son recibidas con la respuesta del servidor. El poder de la promesas es que podemos encadenarlas, transformando los datos que son retornados, y incluso retornar una promesa para un completamente diferente objeto diferido. Como se describe en el Capítulo 3, Comunicación con el Servidor Back-end, AngularJS le permite crear interceptores que puede trabajar con la respuesta del servidor, antes de ser recibida por la llamada original.

### Interceptores de respuesta HTTP

Un interceptor de respuesta (`response`) es una función simple que recibe un objeto prometido para una respuesta del servidor, y a continuación retorna un objeto prometido para el mismo. En cada solicitud `$http`, el objeto promesa respondido será pasado a cada interceptor, a su vez, les da a cada uno una oportunidad de modificar el objeto promesa, antes de devolverla a la llamada original.

Generalmente, una función interceptora usará una llamada a `then()` para encadenar controladores en el objeto promesa que se transmite, y entonces crear una nueva promesa, que a continuación, devuelve. Dentro de esos controladores, podemos leer y modificar el objeto response, tal como cabeceras y datos, como se muestra en el siguiente ejemplo:

```
function myInterceptor(promise) {
 return promise.then(function(response) {
   if ( response.headers()['content-type'] == "text/plain") {
     response.data = $sanitize(response.data);
   };
   return response;
 });
}
```

Este interceptor comprueba el content-type del objeto response. Si este es `text/plain`, desinfectamos los datos del objeto response, y luego retornamos el objeto promise de la respuesta.

### Creando un servicio securityInterceptor

Crearemos un servicio `securityInterceptor` que trabajara con el objeto prometido respondido por el servidor. En nuestro interceptor, comprobamos si el objeto prometido es rechazado con un error 401 No autorizado. En ese caso podemos crear una nueva promesa para reintentar la solicitud original, y luego retornar aquello para la llamada, en lugar de la original.  

> `NOTA` La idea original para esto vino de una excelente publicación de blog por Witold Szczerba: http://www.espeo.pl/2012/02/26/authentication-in-angularjs-application

Crearemos el `securityInterceptor` como un servicio, y luego lo añadiremos a el array responseInterceptors del servicio `$http`.

```
.config(['$httpProvider', function($httpProvider) {
 $httpProvider.responseInterceptors.push(
   'securityInterceptor');
}]);
```

> `NOTA` Tenemos que añadir el servicio `securityInterceptor` por nombre, en lugar de el objeto en sí, porque depende de los servicios que no están disponibles en el bloque de configuración.

Para nuestro servicio `securityInterceptor` implementamos el interceptador como un servicio. de manera que podamos tener otros servicios inyectados en el.  

`NOTA` No podemos inyectar `$http` directamente en nuestro interceptador, porque crearía una dependencia circular, En su lugar, se inyecta el servicio `$injector`, y entonces usarlo para acceder al servicio `$http` al momento de la llamada.

```
.factory('securityInterceptor',
 ['$injector', 'securityRetryQueue',
   function($injector, queue) {
     return function(promise) {
       var $http = $injector.get('$http');
       return promise.then(null, function(response) {
         if(response.status === 401) {
           promise = queue.pushRetryFn('unauthorized-server',
             function() {return $http(response.config); }
           );
         }
         return promise;
       });
     };
   }])
```

Nuestro interceptador esta atento a las respuestas de error que tienen un estado 401. Lo hace proporcionando un controlador para el segundo parámetro de la llamada a `then()`. Al proporcionar `null` para el primer parámetro, indicamos que no estamos interesados ​​en la interceptación del objeto prometido si este es resuelto satisfactoriamente.

Cuando una solicitud falla con un error 401, el interceptador crea una entrada en el servicio `securityRetryQueue`, el cual se describe más adelante. Este servicio repetirá la solicitud fallida, cuando la cola es procesada después de un inicio de sesión satisfactorio.

Lo importante es darnos cuenta de que los controladores de promesas puede retornar cualquiera de los dos: un valor o un objeto promesa:

- Si el controlador retorna un valor, el valor es pasado directo al siguiente controlador en la cadena.
- Si el controlador retorna un objeto promesa, el próximo controlador en la cadena no es desencadenado hasta que esta nuevo objeto promesa ha sido resolvido (o rechazado)

En nuestro caso, cuando la original respuesta retorna un error 401 No autorizado, actualmente retornamos una nueva promesa de reintentar, para reintentar un elemento en servicio en lugar del original objeto promesa para la respuesta. Este nuevo reintento del objeto prometido será resolvido, si el `securityRetryQueue` es reintentado y una nueva respuesta satisfactoria es recibida del servidor, o rechazada si el `securityRetryQueue` es cancelado, o la respuesta es otro error.        

Mientras nuestra llamada original se sienta pacientemente esperando por alguna respuesta del servidor, podemos hacer surgir un caja de inicio de sesión, permitiendo al usuario autenticarse, y luego eventualmente reintentar los elementos en la cola. Una vez que la llamada original recibe una respuesta satisfactoria, es capaz de seguir adelante, como si hubieran sido autenticado al principio.

### Creando el servicio securityRetryQueue

El servicio `securityRetryQueue` proporciona un lugar para almacenar todos esos elementos que serán necesarios reintentar, una vez que el usuario se autentifique satisfactoriamente. Se trata básicamente de una lista, a la que se agrega una función (a ser llamada, cuando el elemento es reintentado) con `pushRetryFn()`. Nosotros procesamos los elementos en la lista llamado `retryAll()` o `cancelAll()`. Aquí está el método `retryAll()`:

```
retryAll: function() {
 while(retryQueue.length) {
   retryQueue.shift().retry();
 }
}
```

Todos los elementos en la cola deben tener dos métodos, `retry()` y `cancel()`. El metodo `pushRetryFn()` hace facil establecer estos objetos, como se muestra en el siguiente código:

```
pushRetryFn: function(reason, retryFn) {
 var deferredRetry = $q.defer();
 var retryItem = {
   reason: reason,
   retry: function() {
     $q.when(retryFn()).then(function(value) {
       deferredRetry.resolve(value);
     },function(value) {
       deferredRetry.reject(value);
     });
   },
   cancel: function() {
     deferredRetry.reject();
   }
 };
 service.push(retryItem);
 return deferredRetry.promise;
}
```

Esta función retorna una promesa para reintentar la función proveedora `retryFn`. Esta reintento del objeto prometido será recibida o rechazada, cuando el elemento en la cola es retirado.

> `NOTA` Tenemos que crear nuestros propios nuevos objetos diferidos para este objeto promesa a reintentar debido a que su resolución o rechazo se dispara, no por la respuesta desde el servidor, sino por la llamada a `retry()` o `cancel()`.

### Notificar al servicio de seguridad

La última pieza en el rompecabezas es como notificar al servicio de seguridad, cuando un nuevo elemento es añadido. En nuestra implementación, simplemente exponemos un método en el `securityRetryQueue` llamado `onItemAdded()`, cual es llamado cada vez que insertamos un elemento en la cola de la siguiente manera:

```
push: function(retryItem) {
 retryQueue.push(retryItem);
 service.onItemAdded();
}
```

El servicio `security` reemplaza o reescribe este método con su propio, de modo que pueda reaccionar cada vez que hay errores de autorización, tal como figura en el siguiente código:  

```
securityRetryQueue.onItemAdded = function() {
 if (securityRetryQueue.hasMore() ) {
   service.showLogin();
 }
};
```

Este código se encuentra en el servicio `security`, donde la variable `security` es el servicio  security en sí.

## Evitar la navegación a rutas seguras.

Evitar la navegación a rutas seguras usando código del lado del cliente no es seguro. La única forma segura de garantizar que los usuarios no puedan navegar a áreas no autorizadas de la aplicación es exigir que se vuelva a cargar la página; esto proporciona al servidor la oportunidad de rechazar el acceso a la URL. Recargar la pagina no es ideal. porque acaba con muchos de los beneficios de las aplicaciones de cliente enriquecido.

> `NOTA` Si bien volver a cargar la página para hacerla segura no es usualmente una buena idea en una aplicación de cliente enriquecido. Puede ser útil, si tienes una clara distinción entre las áreas de su aplicación. Por ejemplo, si su aplicación era en realidad dos subaplicaciones, cada una teniendo requerimientos de autenticación muy diferentes, usted podría acogerlos en diferentes URLs,  y el servidor podría garantizar que el usuario tenga los permisos necesarios antes de permitir que cada subaplicación cargue.
  
En la práctica, desde que podemos asegurar que datos son desplegados al usuario, no es tan importante bloquear el acceso a las rutas usando redirecciones al servidor. En lugar de ello, podemos simplemente bloquear la navegacion a rutas no autorizadas en el cliente cuando la ruta cambia.

> `NOTA` Esto no es una característica de seguridad. Es realmente justo una forma de ayudar a los autenticar correctamente, si ellos navegan directamente a la URL que requiera alguna autorización.

### Usar funciones resolve de ruta

Cada ruta definida con el servicio proveedor `$routeProvider` puede contener un conjunto de funciones `resolve` de ruta. Cada una de estas es una función que puede retornar un objeto promesa, que deben ser resueltas antes de la navegación para la ruta pueda tener éxito. Si cualquiera de los objetos promesa es rechazado, entonces se cancela la navegacion a esa ruta.   

Un enfoque muy simple para la autorización sería proporcionar una función `resolve` de ruta que sólo se resuelva con éxito si el actual usuario tiene la necesaria autorización.

```
$routeProvider.when('/admin/users', {
 resolve: [security, function requireAdminUser(security) {
   var promise = service.requestCurrentUser();
   return promise.then(function(currentUser) {
     if ( !currentUser.isAdmin() ) {
       return $q.reject();
     }
     return currentUser;
   });
 }]
});
```

Aquí, solicitamos una promesa para el actual usuario del el servicio `security`, y entonces la rechazamos si el usuario no es un administrador. El problema con este método es que a el usuario no se le da una oportunidad de iniciar sesion y proporcionar autorización. La ruta es simplemente bloqueada.

De la misma manera que tratamos con los errores de autorización HTTP 401 del servidor. Podemos también reintentar autorizaciones fallidas, al navegar a una ruta. Todo lo que necesitamos es añadir un elemento de reintento al servicio `securityRetryQueue`,  siempre que falle un `resolver` de ruta, intentará la resolución de nuevo, una vez que el usuario haya iniciado sesión.

```
function requireAdminUser(security, securityRetryQueue) {
 var promise = security.requestCurrentUser();
 return promise.then(function(currentUser) {
   if ( !currentUser.isAdmin() ) {
     return securityRetryQueue.pushRetryFn('unauthorized-client',requireAdminUser);
   }
 });
}
```

Ahora, si un usuario no administrador intenta acceder a una ruta, que cuenta con este método `resolve`, un nuevo elemento se agrega al servicio `securityRetryQueue`. Agregar este elemento a la cola desencadenará el servicio `security` que desplegará el formulario de inicio de sesión, donde el usuario puede iniciar sesión con credenciales administrativos. Una vez que el inicio de sesión tenga éxito, el método `requireAdminUser` será juzgado de nuevo y, si tiene éxito, le será permitido realizar el cambio de ruta.

### Crear el servicio de autorización

Para soportar estos métodos `resolve` de ruta, creamos un servicio, llamado `authorization`, el cual proporciona métodos para comprobar si el actual usuario tiene los permisos específicos.

> `NOTA` En una aplicación más compleja, podríamos crear un servicio que pueda ser configurado con un rango de roles y permisos para soportar los requerimientos de seguridad de la aplicacion.

Para nuestra aplicación, este servicio es muy simple, y solo contienen dos métodos: `requireAuthenticatedUser()` y `requireAdminUser()`, los cuales pueden ser descritos de la siguiente manera:

- `requireAuthenticatedUser()`: Este metodo retorna una promesa que solo será resuelta cuando el usuario haya iniciado sesión satisfactoriamente.
- `requireAdminUser()`: Este método retorna una promesa que solo será resuelta cuando un administrador haya iniciado sesión satisfactoriamente.

Ya que estos metodos estan en un servicio, el cual no esta disponible directamente  durante la configuración de `$routeProvider`, normalmente tendríamos que tener que llamar estos métodos en una `funcion`, envueltos en una matriz de la siguiente forma.

```
['securityAuthorization', function(securityAuthorization) {
 return securityAuthorization.requireAdminUser();
}]
```

Para evitar tener que repetir, podemos actualmente colocar esta matriz dentro de un proveedor para el servicio de autorización, como se da en el siguiente código:

```
.provider('securityAuthorization', {
 requireAdminUser: [
   'securityAuthorization',
   function(securityAuthorization) {
     return securityAuthorization.requireAdminUser();
   }
 ],
 $get: [
   'security',
   'securityRetryQueue',
   function(security, queue) {
     var service = {
       requireAdminUser: function() {
       },
     };
     return service;
   }
 ]
});
```

El actual servicio de autorización aparece en la propiedad `$get`. Colocamos los ayudantes resolve de ruta, tal como `requireAdminUser()`, como métodos en el proveedor. En la configuración de nuestras rutas, simplemente inyectamos el proveedor, y luego usamos estos métodos de la siguiente manera:

```
config([
 'securityAuthorizationProvider',
 function (securityAuthorizationProvider) {
   $routeProvider.when('/admin/users', {
     resolve: securityAuthorizationProvider.requireAdminUser
   });
 }
])
```

Ahora nuestras rutas son comprobadas antes de navegar a ellas, y al usuario se le da una oportunidad de autenticar con las credenciales adecuadas, si es necesario.

## Resumen

En este capítulo, hemos visto algunos problemas de seguridad comunes en las aplicaciones web de cliente enriquecido, y cómo se comparan con las tradicionales aplicaciones web basada en el servidor. En particular, mientras que los controles de seguridad siempre se deben realizar en el servidor, el cliente y el servidor deben trabajar juntos para prevenir ataques maliciosos. Implementamos un número de servicios y directivas para soportar la seguridad en nuestra aplicación. Vimos cómo el servicio `$http` basado en promesas de AngularJS nos permite interceptar respuestas a solicitudes al servidor no autorizadas, y luego dar al usuario la oportunidad de autenticar sin tener que interrumpir o reiniciar el flujo de la lógica de la aplicación. Finalmente, hicimos uso de las funciones resolve de ruta en las rutas de nuestra aplicación para comprobar la autorización, antes de permitir al usuario navegar a partes restringidas de nuestra aplicación.

Ahora vamos a ver cómo podemos enseñar a nuestro navegador algunos trucos nuevos desarrollando nuestras propias directivas, las cuales nos permitirán desarrollar componentes de la interfaz de usuario de una manera más declarativa.