## Capítulo 6. Navegación Organizada

En capítulos anteriores, aprendimos cómo extraer datos desde un back-end, editar esos datos usando formularios potenciados por AngularJS, y desplegar los datos en páginas individuales implementando varias directivas. En este capítulo, veremos como organizar pantallas separadas en una aplicación completamente funcional, y fácil de navegar.   
   
Las URLs (Localizadores de recursos uniformes) bien diseñadas y fáciles de recordar juega un vital rol en la estructuración de un aplicación para nuestros usuarios. Estas les permiten moverse efectivamente entre diferentes pantallas, usando las características con las que se sienten cómodos.  AngularJS incluye varios servicios y directivas que traen soporte a las URLs encontradas en las aplicaciones web 1.0 para las aplicaciones web de una sola página, los más notables son los siguientes:

- Deep-linking URLs se refiere a una específica característica en aplicaciones web de una sola página. Ellos pueden ser marcados o pasados alrededor (por ejemplo en un correo o una mensaje instantáneo).
- Los botones adelante y atrás en un navegador se comportan como se esperan, permitiendo a los usuarios moverse entre las diferentes pantallas de la aplicación web de una sola página.
- Las URL pueden tener un excelente formato fácil de recordar en los navegadores que soportan la API history HTML5.
- El soporte de url en AngularJS es consistente a lo largo de los navegadores, el mismo código de la aplicación funciona correctamente en los navegadores con soporte completo para la API history HTML5 así como en navegadores viejos.

Angular tiene sofisticada maquinaria para manejar URLs. En este capítulo, vamos a aprender sobre los siguiente tópicos:

- El uso de las URLs en las aplicaciones web de una sola página.
- El enfoque de AngularJS para las URLs y sus abstraciones sobre las URLs: Los servicios `$location` y `$anchorScroll`.
- Organizando la navegación en una aplicación web en el lado del cliente usando el servicio `$route` (con su proveedor- `$routeProvider`) y la directiva `ngView`
- Patrones comunes, tips, y trucos cuando uses URLs en aplicaciones web AngularJS potenciada de una sola página.

## URLs en aplicaciones web de una sola-página.

La navegación entre páginas fue sencilla en los primeros días de la web. Uno podía escribir una URL en la barra de direcciones del navegador para recuperar un preciso recurso indefinido. Después de todo, un URL es usada para apuntar un solo, recurso físico (un archivo) en un servidor. Cuando una página fue cargada, podemos seguir hipervínculos para saltar entre recursos como también usar los botones adelante y atrás del navegador para movernos entre los elementos visitados.

El aumento del las páginas representadas dinámicamente rompen ese paradigma. Todas de repente, la misma URL podía resultar en diferentes páginas de ser enviado a un navegador, dependiendo del estado interno de la aplicación. Los botones adelante y atrás fueron las primeras víctimas de las web muy interactivas. Su uso se convirtió impredecible, y muchos sitios web siguen yendo tan lejos como desalentar el uso de los botones de navegación adelante y atrás ( animando a los usuarios a confiar en links de navegación internos)

Las aplicaciones web de una sola página, no mejoró la situación, ni mucho menos! En las modernas, aplicaciones AJAX pesadas, no es común ver un sola URL en las barras de los navegadores (la que fue usada para cargar inicialmente la aplicación). Las subsiguientes interacciones HTTP suceden a través del objeto XHR, sin afectar la barra de direcciones del navegador. En esta configuración los botones adelante y atrás son completamente inservibles, ya que precionadolos nos llevarían a sitios web completamente diferentes, en vez de un lugar dentro de la aplicación. Marcar o copiar y pegar un link de la barra de los navegadores no sirve de mucho. Un link marcado siempre apuntará a la página principal de una aplicación.      

Pero los botones adelante y atrás del navegador y la habilidad de marcar URLs es muy útil. Los Usuarios no quieren renunciar a ellos, mientras trabajan con una aplicación web de una sola página! Afortunadamente, AngularJS esta bien equipado para ayudarnos a manejar URL con la misma eficiencia como en los buenos, viejos tiempos de los estáticos recursos de la web.

### Hashbang URLs en la era pre-HTML5

Resulta que hay un truco que puede restaurar un decente soporte para las URLs en las aplicaciones web AJAX pesadas.

El truco está basado en el hecho de que podemos modificar parte de la url en la barra de dirección del navegador, aquella después del carácter # sin desencadenar una recarga de la actual página desplegada. Esta parte de la URL es llamada fragmento de la URL. Cambiando el fragmento de la URL, podemos añadir nuevos elementos a la pila histórica del navegador (el objeto `window.history`). Los botones adelante y atrás confían en el historial del navegador. Así que si conseguimos obtener el historial correctamente, los botones de navegación funcionaran como se espera.

Vamos a considerar un conjunto típico de URLs que a menudo son usadas en las aplicaciones tales como CRUD. Idealmente, nos gustaría tener una URL apuntado a una lista de elementos, un formulario de edición para un elemento, un formulario para nuevos elementos, y así en adelante. Tomando la administración de usuarios (de nuestro ejemplo de aplicación SCRUM), como ejemplo, podríamos típicamente tener las siguiente distintas URL parciales:

- `/admin/users/list` – Esta URL muestra una lista de los usuario existentes.
- `/admin/users/new` – Esta URL muestra un formulario para añadir una nuevo usuario
- `/admin/users/[userId]` – Esta URL muestra un formulario para editar un existente usuario con el ID igual a [ userId ]

Podemos traducir esa URLs parciales a completas URLs con fragmentos en una aplicación web de una sola página usando el truco # como a continuación se muestra:

- `http://myhost.com/#/admin/users/list`
- `http://myhost.com/#/admin/users/new`
- `http://myhost.com/#/admin/users/[userId]`

La URLs que apuntan a parte internas de una aplicación web de una sola página, de esta manera se refieren a menudo como "hashbang URLs".

Teniendo establecido el presente esquema de URL, podemos cambiar la URL en la barra de direcciones del  navegador sin recargar completamente la página . El navegador tomará diferentes URLs (con el mismo prefijo pero con diferente fragmentos URL luego del carácter `#`) para manejar  botones adelante/atrás y su historial.  Al mismo tiempo, no emitirá ninguna llamada al servidor si la parte que solo cambia es el fragmento URL.

Desde luego, cambiar el esquema de la URL no es suficiente. Necesitamos tener una lógica JavaScript que observará cambios de fragmento de la URL, y modificar en consecuencia el estado de la aplicación del lado del cliente.  

### HTML5 y la API Historia

Como usuarios, amamos las URLs simples, memorables, y fácil de recordar, pero el truco hashbang recientemente describido, hace las URLs excesivamente largas y para ser francos algo feas. Afortunadamente hay una especificación HTML5 que soluciona este problema: La API history HTML5.

> `NOTA` La API es soportada en las mayoría de los navegadores modernos. Cuando vino para Internet Explorer, esta es solo incorporada empezado desde la Versión 10. Las versiones anteriores funcionan solo con hashbang URLs.

En corto, usar la API history, podemos simular visitar recursos externos sin actualmente hacer solicitudes al servidor. Podemos hacer eso empujado URLs bien formateadas sobre la pila histórica usando el nuevo método history.pushState. La API history tambien tiene un mecanismo integrado para observar cambios en la pila histórica. Podemos escuchar a el evento `window.onpopstate` y cambiar la el estado de la aplicación en respuesta a esta evento.

Usando la API history HTML5, podemos una vez nuevamente trabajar con excelentes URLs (sin el truco `#`) en una aplicación web de una sola página y disfrutar una experiencia de usuario buena usando URLs marcables, botones adelante y atrás trabajen como se espera, etc. Las URLs del ejemplo anterior pueden ser simplemente representadas de la siguiente manera:

-  http://myhost.com/admin/users/list
-  http://myhost.com/admin/users/new
-  http://myhost.com/admin/users/[userId]

Esas URLs lucen mucho a las URLs “standard”, apuntando a recursos reales en un servidor. Esto es bueno, ya que queremos tener URLs bien parecidas o elegantes. Pero si una de esas URLs es introducida dentro de una barra de direcciones del navegador, un user agent no puede distinguir entre esta URL de cualquier otra URL y emitirá una solicitud al servidor.

> `NOTA` Para que modo HTML5 trabaje correctamente en tales situaciones, el servidor necesita ser configurado apropiadamente, y cooperar siempre retornado la aplicación a la página de destino. Vamos a cubrir la configuración del servidor para el modo de URLs HTML5 más adelante en este capítulo.

## Usando el servicio $location

Angular proporciona una capa de abstracción sobre las URLs (y su comportamiento) en el forma del servicio `$location`. Este servicio disfraza la diferencia entre el hashbang y el modo de URL HTML5 permitiéndonos, a los desarrolladores de aplicaciones, trabajar con una consistente API independientemente del navegador y del modo usado. Pero el servicio `$location` hace más, proporcionado las siguiente funciones:

- Proporciona  conveniente API, para acceder fácilmente a partes de la actual URL (protocol, host, port, path, query parameters, etc).
- Nos permite programáticamente manipular diferentes partes de la actual URL y tener esos cambios reflejados en la barra de dirección del navegador.
- Nos permite observar cambios en diferentes componentes de la actual URL y tomar esas acciones en respuesta a esos cambios.
- Interceptar las interacciones de los usuarios con links en una página (tales como hacer click en una etiqueta `<a>`) para reflejar esas interacciones en el historial del navegador.

### Entendiendo la URLs y API del servicio $location

Antes de ver ejemplo prácticos usando el servicio `$location`, necesitamos familiarizarnos con su API. Ya que el servicio `$location` trabaja con las URLs, la forma facil de empezar es ver como los diferentes componentes de una URL son mapeados por los métodos de la API.  

Vamos a considerar una URL que apunta a una lista de usuarios. Para hacer el ejemplo más complejo, vamos a usar una URL que tiene todos los posible componentes: un path, un query string, y un fragmento. La parte esencial de la URL puede lucir de la siguiente forma:

```
/admin/users/list?active=true#bottom
```

Podemos descifrar esta URLs como: en la sección administrativa, lista todos los usuarios que están activos y desplázate hasta el final de la lista recuperada. Esta es solo la parte de la URL que es interesante para el punto de vista de nuestra aplicación, pero en realidad una URL es “todo el paquete”, con un protocolo, host, etc. Esta URL en su forma completa, será representada diferentemente, dependiendo en el modo (hashbang or HTML5) usado. En la siguiente URL, podemos ver ejemplos de la misma URL representada en ambos modos. En HTML5 lucirá de la siguiente forma:   

```
http://myhost.com/myapp/admin/users/list?active=true#bottom
```

En modo hashbang será más larga y fea, como a continuación:

```
http://myhost.com/myapp#/admin/users/list?active=true#bottom
```

Independientemente del modo usado, la API del servicio `$location` disfrazará las diferencias y ofrecerá  una consistente API. La siguiente tabla muestra un subconjunto de métodos disponibles en la API:

![chapter-6-1](/img/chapter-6-1.png)

Todos estos métodos son getters estilo jQuery. En otras palabras, ellos pueden ser usados para ambas acciones obtener y establecer  (get and set) los valores de los componentes de una determinada URL.  Por ejemplo, para leer el valor fragmento de la URL usarias: `$location.hash()`, mientras que establecer el valor requiere que un argumento sea suministrado a la misma función: `$location.hash('top')`.

El servicio `$location` ofrece otros métodos (no listados en la tabla previa) para acceder a cualquier parte de una URL: el protocolo `protocol()`, el host `host()`, el puerto `port()`, y la URL absoluta `absUrl()`. Los métodos son solamentes getters. No pueden ser usados para mutar la URL.

### Los valores hash, navegación dentro de una página, Y $anchorScroll

El efecto secundario del uso de URLs hashbang es que parte de la URL luego del signo `#`, normalmente usado para navegar dentro de un documento cargado, es “tomado” para representar la interesante parte de la URL de un punto de vista de una aplicación web de una sola página. Sin embargo, hay casos donde necesitamos desplazarnos a un lugar específico en un documento cargado en el navegador. El problema es que, en el modo hashbang las URLs contendría dos signos `#`, por ejemplo:    

```
http://myhost.com/myapp#/admin/users/list?active=true#bottom
```

Los navegadores no tiene manera de saber que el segundo hash (`#bottom`) debe ser usado para navegar dentro del documento, y nosotros necesitamos un poco de ayuda de AngularJS aquí. Esta es donde el servicio `$anchorScroll` viene a jugar.

Por defecto el servicio `$anchorScroll` observará los fragmentos de la URL. Al detectar un hash que debe ser usado para navegar en dentro del documento, se desplazará a la posición específica. Este proceso trabajará correctamente en ambos, el modo HTML5 (donde solo un hash esta presente en la URL) como también en modo hashbang (donde dos hash pueden estar dentro una URL). En corto, el servicio `$anchorScroll` hace el trabajo que es normalmente realizado por el navegador, pero tomando el modo hashbang en cuenta.

Si queremos tener más control sobre el comportamiento de desplazamiento del servicio `$anchorScroll` tu puedes excluirse de su automática observación sobre el fragmento URL. Para hacer eso, necesitas llamar el método `disableAutoScrolling()` en el servicio `$anchorScrollProvider` en un bloque de configuración de un módulo como a continuación:      

```
angular.module('myModule', [])
 .config(function ($anchorScrollProvider) {
   $anchorScrollProvider.disableAutoScrolling();
 });
```

Al hacer estos cambios de configuración, ganamos el control manual sobre cuando el desplazamiento se lleva a cabo. Podemos desencadenar el desplazamiento invocando la función del servicio (`$anchorScroll()`) en cualquier punto de tiempo elegido.

### Configurando el modo HTML5 para las URLs.

Por defecto AngularJS es configurado para usar el modo hashbang para las URLs. Para disfrutar de las URLs de aspecto agradable en modo HTML5, necesitamos cambiar la configuración por defecto de AngularJS, como también configurar nuestro servidor para soportar URLs marcables.

### Lado del cliente

Cambiar al modo de URLs HTML5 en AngularJS es trivial. Todo se reduce a llamar el método `html5Mode()` en `$locationProvider` con un argumento apropiado como de la siguiente manera:

```
angular.module('location', [])
 .config(function ($locationProvider) {
   $locationProvider.html5Mode(true);
 })
```

### Lado del servidor

Para que el modo HTML5 trabaje correctamente en todos los casos, necesitamos un poco de ayuda del servidor que es responsable de servir los archivos de la aplicación AngularJS a el navegador. En pocas palabras, necesitamos establecer redirecciones en el servidor web, de modo que las solicitudes a cualquier deep-linking URL de la aplicacion sera respondida con la página de inicio de la aplicación web de una sola página. (la que contiene la directiva `ng-app`).

Para entender porque tales redirecciones son necesarias, vamos a examinar una situación donde un usuario usa una URL marcada (en modo HTML5) apuntando a un producto atrasado de un proyecto específico. Tal URL pudiera lucir de la siguiente forma:    

```
http://host.com/projects/50547faee4b023b611d2dbe9/productbacklog
```

Para el navegador, tal URL luce como cualquiera otra normal URL y el navegador emitirá una solicitud al servidor. Obviamente tal URL tiene sentido en el contexto de la aplicación web de una sola página, en el lado del cliente. El recurso `/projects/50547faee4b023b611d2dbe9/productbacklog`  no existe fisicamente en el servidor y no puede ser generado por el servidor dinámicamente. La única cosa que el servidor puede hacer con tal URL es redireccionarla al punto de inicio de la aplicación. Esto resultará en que la aplicación AngularJS se cargue en el navegador. Cuando la aplicación comience, el servicio `$location` recogerá la URL (aún presente en la barra de direcciones del navegador), y esta es cuando el procesamiento del lado del cliente puede tomar el relevo.

Proporcionar más detalles de como configurar diferentes servidores escapa del ámbito de este libro, pero hay algunas reglas generales que aplican a todos los servidores. En primer lugar, hay aproximadamente tres tipos de URLs que una tipico servidor web necesita manejar:

- URLs apuntando a recursos estáticos (imágenes, archivos CSS, parciales AngularJS, etc).
- URLs para recuperar data del back-end  o solicitudes de modificaciones (por ejemplo, una API RESTful)
- URLs que representan características de la aplicación en modo HTML5, donde el servidor debe responder con la página de destino (típicamente una URL marcada usada, o una URL escrita en la barra de direcciones del navegador).

Ya que es incómodo enumerar todas las URLs que pueden golpear a un servidor web debido al los links en modo HTML5, es probable que lo mejor sea usar un prefijo conocido para ambos: recursos estáticos y URLs usadas para manipular datos. Esta es una estrategia empleada por la aplicación de ejemplo SCRUM donde todos los recursos estáticos son servidos desde la URLs con el prefijo `/static`, mientras que las prefijadas con el prefijo `/databases` son reservadas para manipulación de datos en el back-end. La mayoría de las URLs remanentes son redireccionadas a punto de inicio de la aplicación SCRUM (`index.html`).

### Navegación hecha a mano usando el servicio $location

Ahora que estamos familiarizado con la API del servicio `$location` y su configuración, nosotros podemos colocar el conocimiento recientemente adquirido en práctica, construyendo un muy simple esquema de navegación usando la directiva `ng-include` y el servicio `$location`.

Incluso el esquema de navegación más simple en una aplicación web de una sola página debería ofrecer algunas facilidades fundamentales para hacer la vida más fácil a los desarrolladores. Como mínimo deberíamos ser capaces de realizar las siguientes actividades:

- Definir rutas fácil de mantener de manera consistente.
- Actualizar el estado de la aplicación en respuesta a cambios de la URL.
- Cambiar la URL en respuesta a los usuarios navegando alrededor de la aplicación (haciendo click en un link, usando los botones adelante y atrás en un navegador, etc)

`NOTA` En el resto del este libro, el término enrutamiento (routing) se utiliza para denotar una facilidad que ayuda a sincronizar el estado de la aplicación (ambas, pantalla y el modelo correspondiente) en respuesta a cambios de la URL. Una sola ruta es una colección de metadatos utilizados para hacer la transición de una aplicación a un estado que coincida con una determinada URL.

### Estructurando páginas alrededor de las rutas

Antes de zambullirnos en los códigos de ejemplos, debemos notar que en una típica aplicación web hay “partes fijas” de una página (header, footer, etc) como también “partes en movimiento”, la cuales deben cambiar en respuesta a las acciones de navegación de los usuarios. Teniendo esto en cuenta, podemos organizar el marcado y los controladores en nuestra páginas en tal forma que las partes en movimiento y fijas están claramente separadas. En base a los previous ejemplos de URLs vistos,  podemos estructurar la aplicación HTML de la siguiente forma:

```
<body ng-controller="NavigationCtrl">
<div class="navbar">
   <div class="navbar-inner">
       <ul class="nav">
           <li><a href="#/admin/users/list">List users</a></li>
           <li><a href="#/admin/users/new">New user</a></li>
       </ul>
   </div>
</div>
<div class="container-fluid" ng-include="selectedRoute.templateUrl">
   <!-- Route-dependent content goes here -->
</div>
</body>
```

La parte móvil en el ejemplo precedente esta representada por una etiqueta `<div>` con la directiva `ng-include` referenciando una dinámica URL :  `selectedRoute.templateUrl`. Pero ¿cómo esta expresión consigue ser cambiada basada en cambios de la URL?.

Vamos a examinar parte del JavaScript, teniendo una mirada cerca al controlador `NavigationCtrl`. En primer lugar podemos definir nuestras rutas de la siguiente forma:

```
.controller('NavigationCtrl', function ($scope, $location) {
 var routes = {
   '/admin/users/list': {templateUrl: 'tpls/users/list.html'},
   '/admin/users/new': {templateUrl: 'tpls/users/new.html'},
   '/admin/users/edit': {templateUrl: 'tpls/users/edit.html'}
 };
 var defaultRoute = routes['/admin/users/list'];
 ...
});
```

El objeto `routes` da una básica estructura de la aplicación, mapea todos las posibles plantillas parciales a sus correspondientes URLs. Mirando estas definiciones de rutas, podemos ver rápidamente qué pantallas componen nuestra aplicación y que parciales son usados en cada ruta.

### Asignando rutas a URLs

Teniendo una simple estructura de enrutado en el lugar nos es suficiente. Necesitamos sincronizar la ruta activa con la actual URL. Podemos hacer esto fácilmente, observando el componente `path()` de la actual URL de la siguiente forma:

```
$scope.$watch(function () {
  return $location.path();
}, function (newPath) {
  $scope.selectedRoute = routes[newPath] || defaultRoute;
});
```

Aquí cada cambio en el componente `$location.path()` dará lugar a una búsqueda para una de las rutas definidas. Si una URL es reconocida, una correspondiente ruta será la seleccionada.  De lo contrario, caemos de nuevo a una ruta por defecto.

### Definiendo controladores en parciales de ruta

Cuando una ruta esta correspondida, la correspondiente plantilla (parcial) es cargada, e incluida en la página por la directiva `ng-include`. Como recordarás la directiva `ng-include` crea un nuevo ámbito, la mayoría del tiempo necesitamos establecer este ámbito con algunos datos que tenga sentido para el parcial cargado recientemente: una lista de usuarios, un usuario a ser editado, etc.

En AngularJS, establecer un ámbito con datos y comportamiento es el trabajo del controlador. Necesitamos encontrar una forma de definir un controlador para cada uno de los parciales. Obviamente, el enfoque más simple es usar la directiva `ng-controller` en el elemento principal de cada parcial. Por ejemplo,  un parcial que es responsable de editar usuarios luciría de la siguiente forma:    

```
<div ng-controller="EditUserCtrl">
  <h1>Edit user</h1>
...
</div>
```

La desventaja de este enfoque es que no podemos usar el mismo parcial con diferentes controladores. Hay casos donde sería beneficioso tener exactamente el mismo marcado HTML, y solo cambiar el comportamiento y los datos establecidos detrás de este parcial. Un ejemplo de tales situaciones es un formulario de edición, donde nos gustaría reusar el mismo formulario para ambas acciones: añadir nuevos elementos y editar un elemento existente. En esta situación el marcado del formulario puede ser exactamente el mismo pero los datos y el comportamiento son ligeramente diferentes.

### Los bits que faltan en la navegación hecha a mano

La solución para esto (ad-hoc) presentada aquí no es muy robusta, y no debe ser usada como es en aplicaciones de la vida real, Aunque incompleta, ilustra muchas interesantes características del el servicio `$location`.

Para empezar con,  el servicio `$location` la API proporciona una excelente envoltura alrededor de varias APIs crudas expuestas por los navegadores. No solo esta API es consistente  a través de los navegadores, pero también lidia con diferentes modos de URL (hashbang y historial HTML5) .

En segundo lugar, realmente podemos apreciar las facilidades por AngularJS y sus servicios. Cualquiera que ha tratado de escribir un sistema de navegación similar en vanilla JavaScript podrá ver rápidamente que tanto ofrece el marco. Ahun, hay numerosos bits que pueden ser mejorados! Pero en lugar de continuar con este desarrollo personalizado, basado en el servicio `$location`, vamos a discutir la solución integrada de AngularJS para esos problemas de navegación relacionados: el servicio `$route`.

## Usando los servicios de enrutado AngularJS integrados.

El marco AngularJS tiene un servicio `$route` integrado que puede ser configurado para administrar transiciones de ruta en aplicaciones web de una sola página. Esta cubre todas las características que intentábamos trabajar a mano usando el servicio `$location` y adicionalmente ofrece otras facilidades muy útiles. Vamos a familiarizarnos con estos pasos-por-pasos.   
  
> `NOTA` Desde la versión 1.2, AngularJS tendrá su sistema de enrutado liberado en un archivo separado (`angular-route.js`) con su propio módulo (`ngRoute`). Cuando trabajamos con la última versión de AngularJS, necesitarás recordar incluir el archivo `angular-route.js` y declarar una dependencia en el módulo `ngRoute`.

### Definición de rutas básicas

Antes de zambullirnos entre los escenarios de uso avanzados, vamos a abandonar nuestra nativa implementación convirtiendo nuestra definiciones de rutas a la  sintaxis usada por el servicio `$route`.
  
En AngularJS, la rutas pueden ser definidas durante la fase de configuración de la aplicación usando el servicio `$routeProvider`. La sintaxis usada por el servicio `$routeProvider` es similar a aquel con el cual estábamos jugando en nuestra personalizada implementación basada en el servicio `$location`.

```
angular.module('routing_basics', [])
 .config(function($routeProvider) {
   $routeProvider
     .when('/admin/users/list', {templateUrl: 'tpls/users/list.html'})
     .when('/admin/users/new', {templateUrl: 'tpls/users/new.html'})
     .when('admin/users/:id', {templateUrl: 'tpls/users/edit.html'})
     .otherwise({redirectTo: '/admin/users/list'});
 });
```

El servicio `$routeProvider` expone una API fluente, donde podemos cambiar los métodos de invocacion para definir nuevas rutas (`when`) y establecer una ruta por defecto (`otherwise`).

> `NOTA` Una vez inicializada, una aplicación no puede ser reconfigurada para soportar rutas adicionales (o remover las existentes). Esto está relacionado con el hecho de que los proveedores AngularJS solo pueden ser inyectados y manipulados en los bloques de configuración ejecutados durante el inicio de la aplicación.

En previos ejemplos, puedes ver que la propiedad `templateUrl` era la única para cada ruta, pero la sintaxis ofrecida por el servicio `$routeProvider` es mucho más rica.      

> `TIP` El contenido de una ruta puede ser especificada en línea usando la propiedad `template` de un objeto de definición de una ruta.  

### Desplegando el contenido de la ruta que concuerda

Cuando una de las rutas concuerda con una URL, podemos desplegar el contenido de las rutas (definido como `templateUrl` o `template`) usando la directiva `ng-view`.

La versión basada en el servicio `$location` del marcado, lucia anteriormente de la siguiente forma:

```
<div class="container-fluid" ng-include="selectedRoute.templateUrl">
  <!-- Route-dependent content goes here -->
</div>
```

Con `ng-view`, esto puede ser reescrito de la siguiente forma:

```
<div class="container-fluid" ng-view>
  <!-- Route-dependent content goes here -->
</div>
```

Como puedes ver, simplemente sustituimos la directiva ng-include por ng-view. Esta vez no necesitamos proporcionar un valor de atributo, ya que la directiva ng-view "sabe" que debe mostrar el contenido para la ruta coincidente actual.

### Coincidencia flexibles de rutas

En la implementación nativa, confiamos en un simple algoritmo de coincidencia de ruta, el cual no soporta partes de variables en la URL. En efecto es un poco exagerado llamarlo un algoritmo, estamos simplemente mirando una propiedad en un objeto correspondiente al path de la url (www.dominio.com/path/...). Debido a la simplicidad del algoritmo, estábamos usando un parámetro de solicitud de la URL en la cadena de la solicitud para pasar alrededor el identificador del usuario de la siguiente manera:

```
/admin/users/edit?user={{user.id}}
```

Seria mucho mejor tener URLs donde el identificador del usuario esté integrado como parte de la URL, por ejemplo:

```
/admin/users/edit/{{user.id}}
```

Con el enrutador AngularJS, esto es muy facil de lograr ya que podemos usar cualquier cadena precedida por un signo de dos puntos (`:`) como un comodín. Para soportar un esquema donde el ID del usuario es parte de una URL que podríamos escribir de la siguiente forma:

```
.when('/admin/users/:userid', {templateUrl: 'tpls/users/edit.html'})
```

Esta definición coincidirá con cualquiera URL con una arbitraria cadena en el lugar del comodín `:userid`, por ejemplo:

```
/users/edit/1234
/users/edit/dcc9ef31db5fc
```

Por otra parte, las rutas con un `:userid` vacío, o el `:userid` contienen barras (/) no coincidirá.

> `TIP` Es posible igualar rutas basadas en parámetros que puedan contener rayas verticales, pero en este caso necesitamos usar ligera sintaxis modificada: `*id`. Por ejemplo, podemos usar la sintaxis basada en * para emparejar paths que contengan rayas verticales: `/wiki/pages/*page`. La sintaxis de coincidencia de ruta será extendida en la Versión 1.2 de AngularJS.

### Definiendo rutas por defecto

La ruta por defecto puede ser configurada llamando el método `otherwise`, proporcionado una definición de una ruta, que atrapa todo por defecto. Por favor nota que el método `otherwise` no requiere que cualquier URL sea especificada, como solo puede haber solo una ruta por defecto.

> `TIP` Una ruta por defecto usualmente redirecciona a una de la ya definidas rutas usando la propiedad `redirectTo` en el objeto de definición de ruta.

La ruta por defecto será usada en ambos casos donde no se proporcionó un path como también como para los casos donde una URL inválida (sin cualquier ruta coincidente) desencadene un cambio de ruta.  

### Acessando a valores de parámetros de la ruta.

Vimos que la definiciones de las URLs pueden contener partes de variables que actúan como parámetros. Cuando una ruta es igualada contra una URL, puedes fácilmente accesar los valores de esos parámetros usando el servicio `$routeParams`. En efecto, el service `$routeParams` es un simple objeto JavaScript (un hash), donde las llaves representa nombre de paramentos de ruta y los valores representan cadenas extraídas de la URL coincidente.

Ya `$routeParams` que es un servicio regular, puede ser inyectado en cualquier objeto administrado por el sistema de inyección de dependencia de AngularJS. Podemos ver esto en acción cuando examinamos un ejemplo de un controlador (`EditUserCtrl`) usado para actualizar los datos del usuario (`/admin/users/:userid`) de la siguiente manera:        

```
.controller('EditUserCtrl', function($scope, $routeParams, Users){
  $scope.user = Users.get({id: $routeParams.userid});
  ...
})
```

El servicio `$routeParams` combina los valores de parámetros de ambos, el path URL como también sus parámetros de búsqueda. Este código funcionaría igual de bien para una ruta definida como `/admin/users/edit` con una URL coincidente: `/admin/users/edit?userid=1234` .

### Rehusando parciales con diferentes controladores

En el enfoque adoptado hasta ahora, definimos un controlador responsable de inicializar el ámbito del parcial, dentro de cada parcial usando la directiva `ng-controller`. Pero el sistema de enrutado AngularJS hace posible definir controladores a nivel de la ruta. Colocando el controlador fuera del parcial, estamos desacoplando efectivamente el marcado del parcial del controlador usado para inicializar el ámbito del parcial.

Vamos a considerar un parcial proporcionando una pantalla para editar datos de un usuario:

```
<div ng-controller="EditUserCtrl">
  <h1>Edit user</h1>
  . . .
</div>
```

Podemos modificarlo removiendo la directiva `ng-controller` tal como:

```
<div>
  <h1>Edit user</h1>
  . . .
</div>
```

Lugar que podamos definir el servicio del controlador a nivel de la ruta de la siguiente forma:

```
.when('/admin/users/:userid', {'templateUrl': 'tpls/users/edit.html','controller': 'EditUserCtrl'})
```

Moviendo el controlador al nivel de definición de la ruta, ganamos la posibilidad de reusar el mismo controlador para diferentes parciales y más importante, rehusar los mismos parciales con diferentes controladores. Esta flexibilidad adicional viene a ser muy útil en varias situaciones. Un típico ejemplo puede ser un parcial conteniendo un formulario para editar un elemento. Usualmente queremos tener exactamente el mismo marcado para ambas acciones añadir un nuevo elemento y editar un elemento existente, pero el comportamiento para nuevos elementos puede ser ligeramente diferente en comparación con los existentes (por ejemplo, crear en lugar de actualizar cuando se almacena el elemento).   

### Evitar el parpadeo de la interfaz de usuario en los cambios de ruta

Mientras una aplicación transita entre diferentes pantallas, usualmente necesitamos obtener y desplegar marcado para una nueva pantalla como también extraer los datos correspondientes (el modelo). Resulta que hay dos ligeras diferentes estrategias que podemos usar para representar una nueva pantalla, que son las siguientes:  

- Desplegar nuevo marcado tan pronto como sea posible (incluso si los datos correspondientes no están listos) y repintar la interfaz de usuario a la llegada de los datos del back-end. 
- Asegurarse que todas las solicitudes a el back-end están completas y que los datos están listos ante desplegar el marcado para una nueva ruta.

El primer enfoque es el predeterminado. Para una ruta con ambas propiedades definidas `templateUrl` y `controller`, AngularJS igualará la ruta y representará el contenido del parcial, incluso si los datos solicitado por un controlador no están listos a tiempo. ¡Por supuesto, AngularJS automáticamente repintará el parcial cuando los datos finalmente arriben (y estén unidos al ámbito), pero los usuario pueden notar un efecto de parpadeo no agradable. El parpadeo de la interfaz de usuario sucede debido a que el mismo parcial es representado dos veces en un corto tiempo, primeramente sin datos y entonces nuevamente cuando los datos están listos.

El sistema de enrutado de AngularJS tiene un excelente, soporte integrado para el segundo enfoque cuando la ruta cambia (y la interfaz de usuario es repintada), es pospuesta hasta que ambos, el parcial y todo los datos solicitados, estén listos. Usando la propiedad `resolve` en un objeto de definición de ruta podemos enumerar dependencias asíncronas para un controlador de una ruta.

Para ilustrar el uso básico de la propiedad resolve, vamos a reescribir nuestra ruta "edit user" de la siguiente forma:

```
.when('/admin/users/:userid', {
  'templateUrl': 'tpls/users/edit.html',
  'controller': 'EditUserCtrl',
  'resolve': {
    'user': function($route, Users) {
      return Users.getById($route.current.params.userid);
    }
  }
})
```

La propiedad `resolve` es un objeto, donde las claves declarar nuevas variables que serán inyectadas en el controlador de la ruta. El valor de esas variables son proporcionadas por una función dedicada. Esta función puede tener dependencias inyectadas por el sistema de inyección de dependencia de AngularJS. Aquí estamos inyectando los servicios `$route` y `Users` para recuperar y retornar los datos del usuario.

Estas funciones resolve, pueden retornar simples valores JavaScript, objetos, o promesas. Cuando una promesa es retornada, AngularJS retrasará el cambio de ruta hasta que la promesa es resuelta. Similarmente si muchas funciones resolve retornan promesas, el sistema de enrutado se asegurará de que todas las promesas están resueltas antes de proceder con el cambio de ruta.

> `NOTA` Las funciones en la sección `resolve` de la definición de la ruta pueden retornar promesas. El actual cambio de ruta se llevará a cabo, si y sólo si toda las promesas son satisfactoriamente resueltas.

Una vez que todas esas variables específica de la ruta (definidas en la sección `resolve`) son resueltas, son inyectadas dentro del controlador de la ruta de la siguiente forma:

```
.controller('EditUserCtrl', function($scope, user){
  $scope.user = user;
  ...
})
```

Esta es un patrón extremadamente poderoso, ya que nos permite definir variables que son locales para una ruta dada y tener esas variables inyectadas dentro de un controlador de una ruta. Hay múltiples aplicaciones prácticas para este patrón y en el la aplicación de ejemplo SCRUM lo estamos usando para reusar el mismo controlador con diferentes valores para la variable user (para cualquiera creada en el lugar o recuperada de un back-end). En el siguiente fragmento de código, podemos ver un extracto de la aplicación de ejemplo SCRUM:

```  
$routeProvider.when('/admin/users/new', {
 'templateUrl':'admin/users/users-edit.tpl.html',
 'controller':'UsersEditCtrl',
 'resolve':{
   'user': function (Users) {
     return new Users();
   }
 }
});

$routeProvider.when('/admin/users/:userId', {
 'templateUrl':'admin/users/users-edit.tpl.html',
 'controller':'UsersEditCtrl',
 'resolve':{
   'user': function ($route, Users) {
     return Users.getById($route.current.params.userId);
   }
 }
});
```

Definiendo variables locales a nivel de la ruta (en la sección `resolve`) significa que los controladores definidos como parte de una ruta pueden ser inyectados con aquellas variables locales. Esto mejora en gran medida nuestra capacidad de realizar pruebas unitarias a la lógica de los controladores.  

### Evitar los cambios de ruta

Hay momentos en los que puede que quieras bloquear los cambios de ruta, basado en ciertas condiciones. Por ejemplo, consideremos la siguiente ruta para editar los datos de un usuario:

```
/users/edit/:userid
```

Decidimos que no debe suceder si un usuario con un específico identificador no existe. Una expectativa razonable sería que los usuario de una aplicación no pudieran navegar a una ruta que apunta a un elemento no existente.

Pues resulta que la propiedad `resolve` de una definición de ruta, tiene soporte integrado para bloquear la navegación de la ruta. Si un valor de una de las llaves (keys) de `resolve` es una promesa que es rechazada, AngularJS cancelará la navegacion de la ruta y no cambiará la vista en la interfaz de usuario.

> `NOTA` Si una de las promesas retornadas en la sección `resolve` de una definición de ruta es rechazada el cambio de ruta será cancelada y la interfaz de usuario no será actualizada.

Cabe señalar que la URL en la barra de direcciones del navegador no será revertida si el cambio es cancelado. Para ver lo que significa en la práctica, vamos asumir que una lista de usuarios es desplegada bajo la URL (`/users/list`). Todos los usuarios en la lista puede que tengan links apuntando a su formulario de edición (`/users/edit/:userid`). Hacer click en uno de esos links cambiará la barra de direcciones del navegador (por lo que se convertirá en algo así como `/users/edit/1234`) pero no se garantiza que podamos editar los detalles del usuario (el usuario pudiera haber sido eliminado en el tiempo medio, o no tenemos suficiente derechos de acceso, etc). Si la ruta de navegación es cancelada la barra de dirección del navegador no será revertida y estaremos leyendo `/users/edit/1234`, incluso si la interfaz de usuario esté reflejando el contenido de la ruta (`/users/list`).

> `NOTA` La barra de direcciones del navegador y la interfaz de usuario puede quedar fuera de sincronización si la navegación de la ruta es cancelada debido a una promesa rechazada.

## Limitaciones del servicio $route

Si bien el servicio `$route` esta muy bien elaborado y hace un excelente trabajo en muchas aplicaciones, hay casos donde se queda corto. Esta sección lista esos casos, para que podamos ser conscientes y dispuestos a cambiar el diseño de nuestra aplicación, o desplegar un servicio de enrutamiento personalizado!

> `NOTA` Hay un esfuerzo impulsado por la comunidad en curso para proporcionar un sistema de enrutado poderoso para aplicaciones AngularJS: `ui-router`. La meta es proporcionar soporte para rutas anidadas y rutas que puedan influenciar múltiples rectángulos en una pantalla. A la hora de escribir esto, aún es un trabajo en progreso pero tu puedes seguir su desarrollo en: `https://github.com/angular-ui/ui-router`

### Una ruta corresponde a un rectángulo en la pantalla

Como usted sabe, la directiva `ng-view` es usada para indicar el elemento del DOM cuyo contenido debe ser reemplazado con el contenido definido por la ruta (vía la propiedad `template` o `templateUrl`). Esto inmediatamente nos indica que en el servicio `$route`, una ruta puede describir el contenido para solo un “hueco” en la interfaz de usuario.  

En realidad, hay momentos en los que nos gustaría llenar en múltiples áreas de una pantalla en respuesta a un cambio de ruta. Un típico ejemplo puede incluir un menú que puede ser común para varias rutas y debe cambiar como navegamos de una área de la aplicación a otra. La siguiente figura ilustra esto:

![chapter-6-2](/img/chapter-6-2.png)

Tomando la precedente figura como un ejemplo, nos gustaría mantener el MENU ADMIN si el usuario dentro del area de administracion. La única forma de lograr este esquema de navegación con la actual versión de AngularJS es usar una combinación de la directiva `ng-include` y el truco descrito en la siguiente sección.

### Administrando múltiples rectángulos de la interfaz de usuario con ng-include

Objetos de definición de rutas son objeto JavaScript regulares y pueden contener propiedades personalizadas, además de las interpretadas por AngularJS. Definiendo una propiedad personalizada no interfiere con el comportamiento por defecto de `$route`, pero el sistema de enrutado de AngularJS preservará esas propiedades, para que podamos acceder a ellas más adelante. Dada esta observación, podemos definir propiedades las personalizadas  `menuUrl` y `contentUrl` a nivel de la ruta de la siguiente forma:

```
$routeProvider.when('/admin/users/new', {
 'templateUrl':'admin/admin.tpl.html',
 'contentUrl':'admin/users/users-edit.tpl.html',
 'menuUrl':'admin/menu.tpl.html',
 'controller':'UsersEditCtrl',
 ...
});
```

Entonces, tendríamos que señalar la propiedad templateUrl en un parcial que se hará cargo de incluir el menú y el contenido de los subparciales, de la siguiente forma:

```
<div>
   <div ng-include='$route.current.contentUrl'>
       <!--menu goes here -->
   </div>
   <div ng-include='$route.current.menuUrl'>
       <!--content goes here -->
   </div>
</div>
```

Mientra que con la precedente solución obtenemos el deseado efecto visual, esta resultara en que el elemento DOM `menu` sea re-representado en cada cambio de ruta, incluso si navegamos con la sección administrativa, donde tal repintado es innecesario. Esto no es un problema si el parcial es simple y no necesita datos que extraer del back-end. Pero tan pronto que necesitemos hacer procesos costosos para cada parcial, necesitamos pensar dos veces antes de desencadenar este proceso con cada cambio de ruta.

### No hay soporte para rutas anidadas

Otra limitación del sistema de enrutado es la falta de soporte para rutas anidadas. En términos prácticos, significa que podemos tener solo una directiva `ng-view`. Dicho de otra manera, no podemos usar la directiva ng-view dentro de cualquier parcial referenciado en una objeto de definición de ruta. Esto puede ser problemático en aplicaciones donde las rutas naturalmente forman un jerarquía. En la aplicación de ejemplo SCRUM, hay un conjunto de rutas, que son las siguientes:

- `/projects` : Provee la lista de todos los proyectos.
- `/projects/[project id]/sprints` : Proporciona la lista de carreras para un proyecto dado.
- `/projects/[project id]/sprints/[sprint id]/tasks` : Proporciona la lista de tareas para un carrera data con el proyecto dado.

Esta esquema de navegación forma una jerarquía visual que puede ser describida de la siguiente forma:

![chapter-6-3](/img/chapter-6-3.png)

Al cambiar entre vistas de tareas para diferentes carreras con el mismo proyecto, nos gustaría mantener los datos (y la interfaz de usuario) para un proyecto dado ya que este no cambia. Desafortunadamente, el principio “una ruta es igual a un rectángulo” nos forza a recargar toda la parte dinámica de una pantalla, incluyendo los parciales específicos del proyecto y su datos correspondientes. Esto a su vez significa que tenemos que recuperar el modelo de proyecto en cada cambio de ruta, incluso si no cambia de una ruta a otra!

Si bien podemos jugar con la directiva `ng-include` para tener un anidamiento correcto de los visuales elementos de la interfaz de usuario, no hay mucho que podamos hacer sobre el aumento del número de solicitudes de datos. Solo podemos tratar de asegurarnos que las repetitivas recuperación de datos del back-end sean tan rápidas como sean posibles, o almacenar en caché las llamadas posteriores a un back-end si es conveniente.

## Patrones específicos de enrutamiento, consejos, y trucos.  

Las secciones anteriores de este capítulo nos dieron una buena vista general de la APIs de AngularJS, relacionada a administrar la navegación en aplicaciones web de una sola página. Aquí vamos a ver ejemplos prácticos usando esas APIs, y discutir las mejores prácticas relacionadas.    

## Manejo de links

La etiqueta HTML (a) es la herramienta primaria para crear links de navegación. AngularJS tiene un tratamiento especial para los enlaces y las siguientes subsecciones le guiarán a través de los detalles específicos de AngularJS.   

### Creación de enlaces en los que puede hacer clic

AngularJS viene empaquetado con una directiva `<a>`, la cual previene las acciones por defecto en links cuando el atributo `href` es omitido. Esto nos permite crear elementos en los que puede hacer clic usando la etiqueta `<a>` y la directiva `ng-click`. Por ejemplo, podemos invocar una función definida en un ámbito haciendo clic en un elemento DOM representado con una etiqueta `<a>` de la siguiente manera:

```
<a ng-click='showFAQ()'>Frequently Asked Questions</a>
```

Tener la etiqueta `<a>` sin una acción de navegación por defecto es útil, como muchos marcos CSS usan la etiqueta `<a>` para representar diferentes tipos de elementos visuales, donde una acción de navegación no tiene mucho sentido. Por ejemplo el marco CSS Twitter Bootstrap usa la etiqueta a para representar cabeceras en componentes pestañas y acordeón.

Dado el especial comportamiento de la etiqueta sin el atributo `href`, puede que se pregunte qué método se debe utilizar para crear vínculos de navegación reales. Uno podría argumentar que podemos usar los siguientes métodos equivalentes:

```
<a href="/admin/users/list">List users</a>
```

Otro método es:

```
<a ng-click="listUsers()">List users</a>
```

Cuando el método `listUsers` puede ser definido en un ámbito de la siguiente forma:

```
$scope.listUsers = function() {
  $location.path("/admin/users/list");
};
```

En la práctica, hay algunas diferencias sutiles entre estos dos enfoques. Para empezar con los links creados usando el atributo `href` es más amigable al usuario, ya que los visitantes pueden hacer clic derecho en tales links y escoger abrirlos en una pestaña separada (o ventana) de un navegador. En general, debemos preferir links de navegación creados con el atributo `href`, o incluso mejor con la directiva AngularJS equivalente `ng-href` que hace fácil crear URLs dinámicas.

```
<a ng-href="/admin/users/{{user.$id()}}">Edit user</a>
```

### Trabajando conscientemente con los modos HTML5 Y hashbang

El servicio `$location` en una aplicación AngularJS puede ser configurado con cualquier modo hashbang o HTML5. Esta solicitud necesita ser hecha en un bloque de configuración, y los link necesitan ser creados acorde con la estrategia escogida.   

Si escogemos usar el modo hashbang debemos crear links de la siguiente forma (nota el carácter `#`):

```
<a ng-href="#/admin/users/{{user.$id()}}">Edit user</a>
```

Mientras en modo HTML5 el link sería ligeramente simple tal como la siguiente forma:

```
<a ng-href="/admin/users/{{user.$id()}}">Edit user</a>
```

Como puedes ver de los ejemplos precedentes, todos los links necesitan ser creados en conformidad con el modo establecido en el servicio `$locationProvider`. El problema aquí es que en una típica aplicación hay muchos links entonces si decidimos cambiar la configuración de `$location`, nos veremos obligados a revisar todos los links en toda la aplicación y añadir (o remover) valores hash.

> `TIP` Decida sobre el modo de la URL en la fase temprana de desarrollo de la aplicación, de otra forma puede  que necesite revisar y cambiar todos los link en toda la aplicación.    

### Enlazando páginas externas

AngularJS asume que los links creados con la etiqueta `<a>` son internos a una aplicación, y como tal, debe cambiar el estado interno de la aplicación, en lugar de desencadenar una recarga de la página en el navegador. La mayoría del tiempo esta es la suposición correcta. Pero hay veces cuando queramos proporcionar un link apuntando a un recurso que debe ser descargado por el navegador. En el modo HTML es imposible distinguir, solo mirando los links que apuntan a el estado interno de los destinado a desencadenar una descarga de recursos externos. La solución es usar el atributo target en tal link de la siguiente forma:

```
<a href="/static/form.pdf" target="_self">Download</a>
```

### Organizando definiciones de ruta

En aplicaciones web de gran escala. no es raro tener docenas y docenas de diferentes rutas. Mientras que el servicio `$routeProvider` ofrece una muy agradable, API de estilo fluido, de definiciones de ruta que pueden llegar a ser algo redundante (especialmente cuando la propiedad resolve es usada). Si combinamos un largo número de rutas con la relativa redundancia de cada definición, rápidamente terminaremos manteniendo un enorme archivo JavaScript con varios cientos de líneas de código! Peor aún, tener todas las rutas definidas en un archivo grande significa que esta archivo debe ser modificado con bastante frecuencia por todos los desarrolladores en un proyecto - una receta para crear cuellos de botellas y problemas de fusión (merge) desastrosos.

### Esparciendo la definiciones de ruta entre varios módulos

Con AngularJS, no estamos obligados a definir todas las rutas en un archivo central! Si tomamos el enfoque descrito en el Capítulo 2, Construyendo y Probando donde cada área funcional tiene su propio módulo dedicado, podemos mover rutas referenciadas a una cierta parte de una aplicación al módulo correspondiente.

En el sistema de módulos AngularJS, todos y cada uno de los módulos puede tener su función `config` asociada, donde podamos inyectar el servicio `$routeProvider` y definir rutas. Por ejemplo, en la aplicación de ejemplo el módulo de administración tiene dos submódulos: Uno para administrar usuarios y otro para administrar proyectos. Cada uno de estos submódulos define su propias rutas de la siguiente manera:          

```
angular.module('admin-users', [])
 .config(function ($routeProvider) {
   $routeProvider.when('/admin/users', {...});
   $routeProvider.when('/admin/users/new', {...});
   $routeProvider.when('/admin/users/:userId', {...});
 });

angular.module('admin-projects', [])
 .config(function ($routeProvider) {
   $routeProvider.when('/admin/projects', {...});
   $routeProvider.when('/admin/projects/new', {...});
   $routeProvider.when('/admin/projects/:userId', {...});
 });

angular.module('admin', ['admin-projects', 'admin-users']);
```

Adoptando este enfoque, podemos esparcir la definiciones de rutas a lo largo de diferentes módulos y evitar mantener un archivo gigante que contenga todas las definiciones de ruta.  

### Luchando con la duplicaciones de código en la definiciones de ruta

Como se ha mencionado en la sección anterior, las definiciones de ruta puede llegar a ser algo redundantes cuando la propiedad `resolve` es usada. Pero si damos un vistazo más de cerca a el código que necesita ser escrito para las rutas en cada área funcional, podemos descubrir rápidamente bloques de código y configuración repetidos. Esto es particularmente cierto para las aplicaciones tipo CRUD donde las rutas para diferentes bloques funcionales tiende a seguir el mismo patrón.

Podemos luchar con la duplicación de código en las definiciones de ruta escribiendo un proveedor personalizado envolviendo el servicio `$routeProvider`. Definiendo nuestro propio proveedor, podemos llegar a una API de alto nivel que captura URLs y patrones funcionales en nuestra aplicación.   

Un proveedor personalizado basado en esta idea probablemente varía de una aplicación a otra, por lo que no estamos ofreciendo detalladas, instrucciones paso a paso de como construir tal utilidad aquí. Sin embargo, el ejemplo de aplicación SCRUM contiene un proveedor personalizado que drásticamente reduce la cantidad de código necesario para definir rutas para la funcionalidad CRUD. Su código fuente puede ser encontrado en GitHub, donde toda la aplicación está alojada, pero el siguiente es un ejemplo de una API personalizada en uso:   

```
.config(function (crudRouteProvider) {
   crudRouteProvider.routesFor('Users')
       .whenList({
           users: function(Users) { return Users.all(); }
       })
       .whenNew({
           user: function(Users) { return new Users(); }
       })
       .whenEdit({
           user: function ($route, Users) {
               return Users.getById($route.current.params.itemId);
           }
       });
})
```

Esto es todo lo que se necesita para definir un conjunto completo de rutas de CRUD para una área funcional.

## Resumen

El diseño eficaz de vínculos de navegación dentro de una aplicación es de suma importancia, ya que forma la columna vertebral alrededor de la cual se envuelve toda la aplicación. Tener una sólida estructura de enlaces es importante para nuestros usuarios, de modo que puedan desplazarse fácilmente dentro de una aplicación web. Pero también es importante para nosotros, los desarrolladores, ya que nos ayuda a estructurar y organizar el código

En este capítulo hemos visto que las aplicaciones creadas con AngularJS pueden ofrecer una excelente experiencia de usuario, cuando se trata de vínculos y navegación, una comparable a los días de la web 1.0. En términos prácticos, esto significa que podremos volver a permitir a nuestros usuarios utilizar los botones Atrás y Adelante del navegador para navegar a través de la aplicación. Además de esto, podemos ver bonitas, URLs marcables en la barra de direcciones del navegador.
   
El servicio `$route` (y su proveedor – `$routeProvider`) nos permiten estructurar la navegación en aplicaciones donde solo una para de la pantalla (un rectángulo) debe ser actualizado en respuesta a un cambio de ruta. El servicio `$route` integrado es en gran medida suficiente para muchas aplicaciones, pero si la descrita característica “una ruta es igual a un cambio en un rectángulo” es demasiado limitante para su caso de uso, usted podría considerar mantener un ojo en soluciones alternativas, impulsadas ​​por la comunidad. O si usted es lo suficientemente valiente considere el despliegue de su propio sistema de enrutamiento. Usted debe estar bien preparado para este momento ya que este capítulo se cubría el servicio `$location` un servicio de nivel bajo sobre el cual se construye el servicio `$route`.

Hacia el final del capítulo, nos fuimos a través de una serie de patrones, consejos y soluciones para los problemas que se encuentran comúnmente en aplicaciones de gran tamaño que utilizan enrutamiento. Con suerte, los ejemplos proporcionados en esta sección del capítulo son fácilmente aplicables a su aplicación. En particular, le alentamos a mover las definiciones de ruta a los módulos funcionales correspondientes con el fin de evitar el mantenimiento de un archivo enorme que contenga todas las definiciones de ruta.

Temas relacionados con el enrutamiento y navegación están vinculadas a consideraciones de seguridad. De hecho, en aplicaciones en las que no toda la información es pública tenemos que restringir ciertos usuarios a un subconjunto de rutas disponibles. El siguiente capítulo trata los patrones para asegurar las rutas, así como muchos otros temas en detalles relacionados con la seguridad.