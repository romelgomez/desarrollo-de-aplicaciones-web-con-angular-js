## Capítulo 4. Mostrar y dar formato a datos

Ahora que conocemos como obtener data de un back-end en el navegador podemos enfocarnos en desplegar y manipular la data usando AngularJS. Este capítulo comienza con una visión general de las directivas AngularJS usadas en plantillas. Tan pronto como los fundamentos sean cubiertos veremos diferentes directivas en acción y discutiremos sus patrones de uso. También hay una amplia cobertura de los filtros AngularJS.

En este capítulo aprenderás sobre:

- Convenciones de nombre para las directivas AngularJS
- Soluciones para condicionalmente mostrar y ocultar bloques de marcado.
- Los patrones de uso y las trampas de la directiva repetidora `ng-repeat`.
- Registro de controladores de eventos DOM para que los usuarios pueden interactuar con una aplicación.
- Limitaciones del lenguaje de plantilla basados en DOM de AngularJS y posibles soluciones.
- Filtros: su propósito y ejemplos de uso. Vamos a ir sobre filtros integrados, así como la forma de crear y probar un filtro personalizado.

## Hacer referencia a las directivas

Antes de ir sobre los ejemplos de las diferentes directivas incorporadas AngularJS, es importante destacar que podemos usar una variedad de convenciones de nombre al referenciar directivas en el marcado HTML.

En la documentación de AngularJS todas las directivas son indexadas bajo su nombre camel-case (por ejemplo, `ngModel`). En la plantilla, sin embargo, necesitamos usar otra forma snake-case (`ng-model`), separados por dos puntos (`ng:model`) o separados por subrayado (`ng_model`). Adicionalmente, cada referencia a la directiva puede ser prefijada con x o datos.

> `NOTA` Cada sola directiva puede ser referenciada por un número de diferentes (pero equivalente) de nombres. Tomando la directiva ngModel como un ejemplo podemos escribir en nuestra plantilla una de los siguientes: `ng-model`, `ng:model`, `ng_model`, `x-ng-model`,
`x-ng:model`, `x-ng_model`, `data-ng-model`, `data-ng:model`, `data-ng_model`, `x:ng-model`, `data_ng-model`, y así sucesivamente.

La data prefijada es muy conveniente para mantener nuestros documentos HTML compatibles con HTML5 , y hacer que ellos pasen las pruebas de validación HTML5. Por otro lado eres libre de escoger cualquier esquema de nombre que le plazca; que son todos equivalentes.

> `TIP` Todos los ejemplos en este libro están usando la corta forma snake-case (por ejemplo, `ng-model`). Encontramos esta sintaxis concisa y legible.  

### Desplegando resultados de expresiones de evaluación.

AngularJS ofrece varias maneras de representar datos. El efecto neto es siempre el mismo contenido de modelo es desplegado a los usuarios, pero también hay sutilezas que cabe señalar.

### La directiva de interpolación

La directiva de interpolación es la más fundamental directiva que se ocupa de exhibir los datos del modelo. Se acepta expresiones delimitados por un par de llaves:

```
<span>{{expression}}</span>
```

Esta directiva simplemente evaluará la expresión `expression` y representará su resultado en una pantalla.

El delimitador utilizado por AngularJS es configurable, cambiar los valores predeterminados podría ser necesario si va a mezclar AngularJS con otro lenguaje de plantillas en el lado del servidor. La reconfiguration es realmente simple y se reduce a establecer las propiedades de la `$interpolateProvider`:   

```
myModule.config(function($interpolateProvider) {
  $interpolateProvider.startSymbol('[[');
  $interpolateProvider.endSymbol(']]');
});
```
Aqui estamos cambiando los valores predeterminados `{{}}` a `[[]]` por lo que más adelante podemos escribir en las plantillas:

```
[[expression]]
```

## Representando valores del modelo con ngBing

La directiva de interpolación tiene una directiva equivalente llamada `ng-bing`. Puede ser usada como un atributo HTML:

```
<span ng-bind="expression"></span>
```

La forma de las llaves es más fácil de usar pero hay caso donde la directiva `ng-bind` puede ser útil. Usualmente la directiva ng-bind es usada para esconder expresiones antes  AngularJS tenga el chance de procesarlas en la carga de la página inicial. Esto evita el parpadeo de interfaz de usuario y proporciona una mejor experiencia de usuario. El tópico de optimizar la carga de la página inicial se cubre más detalladamente en el **Capítulo 12, Empaquetando y Desplegando Aplicaciones Web AngularJS**.

### Contenido HTML en expresiones AngularJS

Por defecto AngularJS escapará cualquier marcado HTML que haga dentro de una expresión (modelo) evaluada por la directiva de interpolación. Por ejemplo, dado el modelo:

```
$scope.msg = 'Hello, <b>World</b>!';
```

Y el fragmento de marcado:

```
<p>{{msg}}</p>
```

El proceso de representación escapará las etiquetas `<b>`, por lo que van a aparecer en forma de texto sin formato y no como lenguaje de marcado:

```
<p>Hello, &lt;b&gt;World&lt;/b&gt;!</p>
```

La directiva de interpolación escapara cualquier contenido HTML encontrado el modelo con el fin de prevenir ataques de inyección HTML.

Si por alguna razón, su modelo contiene lenguaje de marcado HTML que necesite ser evaluado y representado por un navegador puede usar la directiva `ng-bind-html-unsafe` para desactivar por defecto el escapado de etiquetas HTML.

```
<p ng-bind-html-unsafe="msg"></p>
```

Usando la directiva `ng-bind-html-unsafe` obtendremos el fragmento HTML con las etiquetas `<b>` interpretadas por el navegador.

Extremo cuidado se debe tomar cuando use la directiva `ng-bind-html-unsafe`. Su uso debe ser limitado a los caso donde confíe completamente o pueda controlar la expresión siendo evaluada. De otra forma usuarios maliciosos pueden inyectar cualquier HTML arbitrariamente en su página.
 
AngularJS tiene una directivas más que selectivamente esterilizan ciertas etiquetas HTML mientras permite a otras ser interpretadas por un navegador: `ng-bind-html`. Su uso es similar a el equivalente inseguro:

```
<p ng-bind-html="msg"></p>
```

En términos de escape la directiva `ng-bind-html` es un compromiso entre el comportamiento de la directiva `ng-bind-html-unsafe` (permite todas las etiquetas HTML) y la directiva de interpolación (no permiten etiquetas HTML en absoluto). Podría ser una buena alternativa para caso donde queramos permitir etiquetas HTML ingresadas por los usuarios.

> `NOTA` La directiva `ng-bind-html` reside en un módulo separado (`ngSanitize`) y requiere la inclusión de un adicional archivo fuente: `angular-sanitize.js`.

No olvide declarar dependencias en el módulo `ngSanitize` si planea la directiva `ng-bind-html`:

```
angular.module('expressionsEscaping', ['ngSanitize'])
  .controller('ExpressionsEscapingCtrl', function ($scope) {
    $scope.msg = 'Hello, <b>World</b>!';
  });
```

> `TIP` A menos que esté trabajando con sistemas legados existentes (CMS, back-ends sending HTML, etc), el lenguaje de marcado en el modelo debe ser evitado, Tal lenguaje de marcado no puede contener directivas AngularJS y requiere la directiva `ng-bind-html-unsafe` o `ngbind-html` para obtener resultados deseados.

### Visualización condicional

Mostrando y escondiendo parte del DOM basados en algunas condiciones es un requerimiento común. AngularJS viene equipado con 4 diferentes conjuntos de directivas para esta ocasión (`ng-show/ng-hide`, `ng-switch-*`, `ng-if` y `ng-include`).

La familia de directivas `ng-show/ng-hide` puede ser usada para ocultar (aplicando reglas de visualización CSS) partes del árbol DOM basándose en un resultado de evaluación de la expresión:

```
<div ng-show="showSecret">Secret</div>
```

El código anterior reescrito usando la directiva `ng-hide` se vería de la siguiente manera:

```
<div ng-hide="!showSecret">Secret</div>
```

> `NOTA` La directivas `ng-show/ng-hide` hacen el truco simplemente aplicando `style="display: none;`  para ocultar elementos DOM. Aquellos elementos no son removidos del árbol DOM.

Si queremos físicamente remover o añadir nodos DOM condicionalmente, la familia de directivas `ng-switch` (`ng-switch`, `ng-switch-when`, `ng-switch-default`) llegarán a ser utiles.

```
<div ng-switch on="showSecret">
  <div ng-switch-when="true">Secret</div>
  <div ng-switch-default>Won't show you my secrets!</div>
</div>
```

La directiva `ng-switch` está muy cerca de la sentencia switch JavaScript como podamos tener varias ocurrencias `ng-switch-when` dentro de una directiva `ng-switch`.

> `NOTA`  La diferencia principal entre las directivas `ng-show/ng-hide` y `ng-switch` es la manera en que los elementos DOM son tratados. La directiva ng-switch añadirá y removerá elementos DOM del árbol DOM mientras que las directivas ng-show/ng-hide simplemente aplicarán el estilo `style="display: none;"` para esconder elementos. La directiva `ng-switch` está creando un nuevo ámbito.

Las directivas `ng-show/ng-hide` son fáciles de usar pero podría tener consecuencias desagradables de rendimiento si es aplicada a un largo número de nodos DOM. Si toca los problemas de rendimiento relacionado a el tamaño del árbol DOM debería aprender el uso más detallado de la familia de directivas `ng-switch`.

El problema con la familia de directivas `ng-switch` es que la sintaxis puede llegar a ser muy detallada para un simple uso de caso. Afortunadamente AngularJS tiene una directiva más en su arsenal: `ng-if`. Se comporta de manera similar a la directiva `ng-switch` (en el sentido que agrega / remueve elementos del árbol DOM) pero tiene una sintaxis muy simple:

```
<div ng-if="showSecret">Secret</div>
```

> `NOTA` La directiva `ng-if` esta disponible solo en la más reciente versión de AngularJS: 1.1.x ó 1.2.x.

### Incluyendo bloques de contenido condicionalmente.

La directiva `ng-include`, mientras no actúe directamente como la declaración `if/else`, puede ser usada para condicionalmente desplegar bloques de lenguaje de marcado potenciado por AngularJS dinámicamente. La directiva discutida tiene una propiedad muy agradable. Ella puede cargar y condicionalmente desplegar parciales basados en un resultado de evaluación de una expresión. Esto nos permite crear fácilmente páginas muy dinámicas. Por ejemplo, podemos incluir diferentes formularios de edición de usuario dependiendo del rol de los usuarios. En el siguiente fragmento de código cargamos diferentes parciales para los usuarios que tenga un rol administrativo.

```
<div ng-include="user.admin && 'edit.admin.html' || 'edit.user.html'"></div>
```

> `NOTA` La directiva `ng-include` está creando un nuevo ámbito por cada parcial que incluye.

Adicionalmente `ng-include` es la super herramienta que puede ser usada para componer páginas finales desde pequeños fragmentos de lenguaje de marcado.

> `TIP` La directiva `ng-include` acepta una expresión como su argumento, por lo que necesita pasar una cadena entre comillas si piensa utilizar un valor fijo apuntando a un parcial, por ejemplo, `<div ng-include="'header.tpl.html'"></div>`.

### Representando colecciones con la directiva ngRepeat

La directiva `ng-repeat` es probablemente una de la más usada y más poderosa. Ella iterará sobre una colección de artículos estampando un nuevo elemento DOM por cada entrada en una colección. Pero la directiva `ng-repeat` hará mucho más que simplemente asegurar la inicial representación de una colección. Constantemente observará la fuente de datos para re-representar la plantilla en respuesta a los cambios.   

> `NOTA` La implementación repetidora esta altamente optimizada y tratará de minimizar el número de cambios DOM necesarios para reflejar la estructura de la data en el árbol DOM.

Internamente la directiva `ng-repeat` podría optar por cambiar nodos DOM alrededor  (si mueve un elemento en el array), borrar un nodo DOM si un elemento es removido de el array y insertar nuevos nodos si elementos adicionales terminar en el array. Independientemente de la estrategia escogida por un repetidor entre bastidores es fundamental tener en cuenta que no es un simple bucle que se ejecutará una vez. La directiva `ng-repeat` se comporta más como un observador de los datos que intenta asignar entradas en una colección de nodos DOM. El proceso de observación de datos es continuo.

### Familiarizarse con la directiva ngRepeat

El uso básico y la sintaxis es muy simple:

```
<table class="table table-bordered">
   <tr ng-repeat="user in users">
       <td>{{user.name}}</td>
       <td>{{user.email}}</td>
   </tr>
</table>
```

Aquí el array users es definido en un ámbito y contiene el típico objeto user con las propiedades como: name, email, etc. La directiva `ng-repeat` iterará sobre la colección de users y creará un elemento DOM `<tr>` por cada entrada en la colección.

> `NOTA` La directiva `ng-repeat` crea un nuevo ámbito por cada elemento de una colección.  

### Variables especiales

El repetidor AngularJS declarará un conjunto de variables especiales en un ámbito creado para para cada individual entrada. Esas variables pueden ser usadas para determinar la posición de un elemento dentro de una colección  

`$index`: Será establecida con un número que indica el índice de un elemento en una colección (el índice comienzan en 0).
`$first, $middle, $last`: De estas variables obtendrá un valor booleano acorde a la posición de los elementos.

La mencionada variables viene a ser muy útiles en muchas situaciones de la vida real. Por ejemplo, en la aplicación de ejemplo SCRUM nosotros podemos confiar en la variable `$last` para representar apropiadamente los link en un elemento breadcrumb. Para el última parte del camino (seleccionado) no hay necesidad de representar un link, mientras que el elemento `<a>` es necesitado para las otras partes de la ruta.

![chapter-4-1](/img/chapter-4-1.png)

Nosotros podemos modelar esta interfaz de usuario con el siguiente código:

```
<li ng-repeat="breadcrumb in breadcrumbs.getAll()">
   <span class="divider">/</span>
   <ng-switch on="$last">
       <span ng-switch-when="true">{{breadcrumb.name}}</span>
       <span ng-switch-default>
           <a href="{{breadcrumb.path}}">{{breadcrumb.name}}</a>
       </span>
   </ng-switch>
</li>
```

### Iteración sobre propiedades de un objeto

Usualmente la directiva `ng-repeat` es usada para desplegar entradas de un array JavaScript. Alternativamente puede ser usado para iterar sobre propiedades de un objeto. En este caso la sintaxis es ligeramente:

```
<li ng-repeat="(name, value) in user">
   Property {{$index}} with {{name}} has value {{value}}
</li>
```

En el ejemplo precedente, podemos desplegar todas las propiedades de un objeto user como una lista desordenada. Por Favor nota que nosotros debemos especificar nombres de variables para ambas, la propiedad y su valor, usando una notación entre paréntesis (`name`, `value`).

> `NOTA` La directiva `ng-repeat` antes de la salida de resultados, clasificará los nombres de propiedad en orden alfabético. Este comportamiento no puede ser cambiado, por lo que no hay manera de controlar el orden de iteración mientras usamos la directiva `ng-repeat` con objetos.

La variable `$index` puede ser usada para obtener una posición numérica de una propiedad dada en una lista ordenada para todas las propiedades.

Iterando sobre propiedades de objetos, mientras se soporta, tiene sus limitaciones. El problema principal es que no podemos controlar el orden de iteración.

> `TIP` Si usted se preocupa por el orden en que las propiedades son itineradas, debería ordenar esas propiedades en un controlador y colocar artículos ordenados en un array.  

### Patrones ngRepeat

Esta sección nos llevará a través de algunos de los patrones de presentación de uso común y la formas de implementarlas con AngularJS. En particular vamos a ver dentro de las listas con detalles y alterando clases en los elementos que forman parte de una lista.

### Las listas y detalles

Es un uso de caso común desplegar una lista cuyos artículos se expande para mostrar detalles adicionales, cuando se hace click sobre ellos. Hay dos variantes para esta patron: Solo un elemento puede ser expandido o alternativamente varios elementos expandidos es permitido. Aqui una captura de pantalla ilustrando esta particular diseño de la interfaz de usuario.     

![chapter-4-2](/img/chapter-4-2.png)

### Desplegando solo una fila con detalles.

El requerimiento de tener solo un elemento expandido puede ser fácilmente cubierto con el siguiente código:

```
<table class="table table-bordered" ngcontroller="
ListAndOneDetailCtrl">
   <tbody ng-repeat="user in users" ng-click="selectUser(user)" ngswitch
          on="isSelected(user)">
   <tr>
       <td>{{user.name}}</td>
       <td>{{user.email}}</td>
   </tr>
   <tr ng-switch-when="true">
       <td colspan="2">{{user.desc}}</td>
   </tr>
   </tbody>
</table>
```

En el ejemplo precedente una adicional fila, contiene los detalles del usuario, solo es representado si un usuario determinado fue seleccionado. Un proceso de selección es muy simple y está cubierto por las funciones `selectUser` y `isSelected`:

```
.controller('ListAndOneDetailCtrl', function ($scope, users) {
   $scope.users = users;
   $scope.selectUser = function (user) {
       $scope.selectedUser = user;
   };
   $scope.isSelected = function (user) {
       return $scope.selectedUser === user;
   };
})
```

La dos funciones aprovechar el hecho de que hay un ámbito (definido arriba de elemento DOM tabla) donde podemos almacenar un apuntador (`selectedUser`) para activar un artículo de una lista.

### Desplegando muchas filas con detalles

Asumiendo que queremos permitir múltiples filas con detalles adicionales necesitamos cambiar la estrategia. Esta vez los detalles de la selección deben ser almacenados en cada nivel de elemento. Como podrás recordar la directiva `ng-repeat` esta creando un nuevo ámbito para todas y todos los elementos de una colección que itera. Podemos tomar ventaja de este nuevo ámbito para almacenar el estado “seleccionado” para cada artículo.   

```
<table class="table table-bordered">
   <tbody ng-repeat="user in users" ng-controller="UserCtrl" ng-click="toggleSelected()" ng-switch on="isSelected()">
       <tr>
           <td>{{user.name}}</td>
           <td>{{user.email}}</td>
       </tr>
       <tr ng-switch-when="true">
           <td colspan="2">{{user.desc}}</td>
       </tr>
   </tbody>
</table>
```

Este ejemplo es interesante ya que estamos utilizando la directiva `ng-controller` para cada artículo. Un controlador proporcionado puede aumentar el ámbito con funciones y variables para controlar el estado de seleción.

```
.controller('UserCtrl', function ($scope) {
   $scope.toggleSelected = function () {
       $scope.selected = !$scope.selected;
   };
   $scope.isSelected = function () {
       return $scope.selected;
   };
});
```

Es importante entender que especificando un controlador en el mismo elemento DOM como la directiva `ng-repeat` significa que el controlador administra el nuevo ámbito creado por el repetidor. En práctica significa que podemos tener una controlador dedicado a administrar individualmente los artículos de una colección. Esto es un poderoso patron que nos permite prolijamente encapsular específico a cada articulo variables y comportamientos (funciones).

### Alterando tablas, columnas, y clases.

Zebra-striping a menudo se añade a listas con el fin de mejorar su legibilidad. AngularJS tiene un par de directivas (`ngClassEven` y `ngClassOdd`) que hace esta tarea trivial:

```
<tr ng-repeat="user in users" ng-class-even="'light-gray'" ng-class-odd="'dark-gray'">
  . . .
</tr>
```

Las directivas `ngClassEven` y `ngClassOdd` son justamente una especialización de la más genérica directiva `ngClass`. La directiva `ngClass` es muy versátil y puede ser aplicada en diferentes situaciones. Para demostrar su poder podemos reescribir el anterior ejemplo de la siguiente manera:

```
<tr ng-repeat="user in users" ng-class="{'dark-gray' : !$index%2, 'light-gray' : $index%2}">
```

Aqui la directiva `ngClass` es usada con un objeto argumentado. Las claves de este objeto son los nombres de clase y valores; y expresiones condicionales.  Una clase especificada como una clave será añadida o removida de un elemento basado en un resultado de una correspondiente evaluación de expresión.

> `TIP` La directiva puede aceptar también argumentos del tipo cadena o array. Ambos argumentos puede contener una lista de clases CSS (separados por coma en caso de una cadena) para ser añadida a un elemento dado.  

## Controladores de eventos DOM

Nuestra interfaz de usuario no sería muy útil si los usuarios no pudieran interactuar con ella (ya sea mediante el uso de un ratón, un teclado o eventos táctiles). Las buenas noticias es que registro de controladores de eventos es un juego de niños con AngularJS! Aqui un ejemplo de reaccionar ante un evento de clic:

```
<button ng-click="clicked()">Click me!<button>
```

La expresión `clicked()` es evaluada contra el actual $scope lo cual hace que sea muy fácil llamar a cualquier método definido en el ámbito. No estamos limitados a una simple llamada a una función; es posible usar expresiones de una complejidad arbitraria, incluyendo las que aceptan argumentos:

```
<input ng-model="name">
<button ng-click="hello(name)">Say hello!<button>
```

>`NOTA` Los desarrolladores nuevo en AngularJS a menudo intenta registrar controladores de eventos de la siguiente manera: `ng-click="{{clicked()}}"` or `ng-click="sayHello({{name}})"` es decir, utilizando la expresión de interpolación dentro de un valor de atributo. Esto no es necesario y funcionara correctamente. Lo que sucedería es que AngularJS analizará y evaluará una expresión de interpolación mientras procesa el DOM. El primera etapa del proceso evaluar una expresión de interpolación y usa el resultado de esta evaluación como un menejador de eventos!.

AngularJS tiene incorporado soporte para diferente eventos con las siguientes directivas:

- Eventos de click: ngClick y ngDblClick
- Eventos de raton: ngMousedown, ngMouseup, ngMouseenter, ngMouseleave, ngMousemove y ngMouseover
- Eventos de teclado: ngKeydown, ngKeyup y ngKeypress.
- Eventos de cambio en campos de entrada (ngChange): La directiva ngChange copera con la directiva ngModel, y nos permite reaccionar a cambios de modelo inicializados por entradas del usuario.

  
Los mencionados controladores de eventos DOM puede aceptar un special argumento $event en su expresión, que representa la raw del evento DOM. Esto nos permite obtener el acceso a las propiedades de bajo nivel de un evento, prevenir su acción por defecto, para su propagación, etc. Como un ejemplo podemos ver como leer la posicion de un elemento clickeado.

```
<li ng-repeat="item in items" ng-click="logPosition(item, $event)">
  {{item}}
</li>
```

Donde la función `logPosition` es definida en un ámbito como el siguiente:  

```
$scope.logPosition = function (item, $event) {
 console.log(item + ' was clicked at: ' + $event.clientX + ',' +
   $event.clientY);
};
```

> `TIP` Mientras que la especial variable `$event` es expuesta al controlador de evento, no se debe abusar de hacer manipulaciones DOM extensivas. Como recordarás del **Capítulo 1, Angular Zen**, en AngularJS todo es sobre la declarativa interfaz de usuario y la manipulación DOM debería ser limitada a las directivas. Esta es la razón por la cual el argumento `$event` es mayormente usado dentro del código de las directivas.

## Trabajando efectivamente con plantillas basadas en DOM.

No es muy común ver un sistema de plantillas implementado la sintaxis HTML y DOM en vivo, pero este enfoque resulta funcionar sorprendentemente bien en la práctica. La personas que usan otro motor de plantillas basado en cadenas puede ser que necesiten un poco de tiempo para adaptarse, pero luego de unos pocos saltos iniciales escribiendo plantillas basadas en DOM se convierte en una segunda naturaleza. Hay sólo un par de advertencias.

### Vivir con una sintaxis detallada.

Primeramente, la sintaxis para algunos constructores puede ser algo detallada. El mejor ejemplo de esta sintaxis ligeramente molesta para la familia de directivas `ng-switch` algunos comunes uso de casos pueden simplemente requerir escribir mucho. Vamos a considerar un simple ejemplo para desplegar un diferente mensaje en base al número de artículos en una lista:

```
<div ng-switch on="items.length>0">
 <span ng-switch-when="true">
   There are {{items.length}} items in the list.
 </span>
 <span ng-switch-when="false">
   There are no items in the list.
 </span>
</div>
```

Por supuesto que sería posible mover la lógica que prepara el mensaje a un controlador para evitar la declaración de switch en la plantilla, pero se siente este tipo de lógica condicional simple tiene su lugar en la capa de vista.

Afortunadamente, la última versión de AngularJS: la directiva `ng-if` incorporada es muy útil para caso donde no necesites el poder de toda la expresión `if/else`.

### ngRepeat y múltiples elementos DOM.

Un tema un poco más serio está conectado con el hecho de que, en su forma más simple, la directiva repetidora `ng-repeat` sólo sabe repetir un elemento (con sus dependientes). Esto significa que la directiva ng-repeat no puede administrar un grupo de elementos hermanos.

Para ilustrar esto vamos a imaginar que tenemos una lista de artículos con un nombre y descripción. Ahora, queremos tener una tabla donde ambos el nombre y la descripción son representados con filas separadas (`<tr>`). El problema es que necesitamos añadir la etiqueta `<tbody>` sólo para tener un lugar para la directiva `ng-repeat`:

```
<table>
 <tbody ng-repeat="item in items">
   <tr>
     <td>{{item.name}}</td>
   </tr>
   <tr>
     <td>{{item.description}}</td>
   </tr>
 </tbody>
</table>
```

Pareciera que la directiva necesita un elemento contenedor y nos forsa a crear un cierta estructura HTML. Esto puede ser o no un problema dependiendo en como estrictamente necesites seguir el lenguaje de marcado delineado por sus diseñadores web

En el previo ejemplo tuvimos suerte ya que existe un contenedor HTML (`<tbody>`), pero hay casos donde no hay un válido elemento HTML donde la directiva `ng-repeat` pueda ser colocada. Podemos pensar en otro ejemplo donde podamos tener una salida HTML tal como:

```
<ul>
 <!-- we would like to put a repeater here -->
 <li><strong>{{item.name}}</strong></li>
 <li>{{item.description}}</li>
 <!-- and end it here -->
</ul>
```

Una nueva versión de AngularJS (1.2.x) va a extender la sintaxis básica de la directiva `ngRepeat` para permitir más flexibilidad al escoger elementos DOM para ser iterados. En el futuro será posible escribir:

```
<ul>
 <li ng-repeat-start="item in items">
   <strong>{{item.name}}</strong>
 </li>
 <li ng-repeat-end>{{item.description}}</li>
</ul>
```

Usando las directivas `ng-repeat-start` y `ng-repeat-end` será posible indicar un grupo de elementos DOM hermanos para ser iterados.

### Elemento y atributos que no pueden ser modificados en tiempo de ejecución

Desde que AngularJS opera en el vivo árbol DOM un navegador puede solo hacer como mucho lo que los navegadores permita. Resulta ser que algunos casos puntuales donde algunos navegadores deshabilitan cualquier cambio a los elementos y sus atributos, luego que esos elementos son colocado en el árbol DOM.

Para ver la consecuencias de esta restricción en la práctica vamos a considerar un raro uso de caso común especificando de un elemento de entrada de dato (input) su atributo type dinámicamente. Muchos usuarios han tratado (y han fallado!) de escribir código que permita esas líneas:

```
<input type="{{myinput.type}}" ng-model="myobject[myinput.model]">
```

El problema es que algunos navegadores (si, has adivinado: Internet Explorer!) no cambia el tipo de un campo de entrada. Esos navegadores ven `{{myinput.type}}` (sin evaluar) como un tipo campo de entrada, y ya que es desconocido es interpretado como `type="text"`.

Hay muchos enfoques para abordar el problema descrito anteriormente, pero necesitamos aprender más sobre las directivas personalizadas AngularJS antes de que podamos discutir esas soluciones. `El Capítulo 9, Construyendo Avanzadas Directivas`, ofrece una de las posibles soluciones. El otro simple enfoque es usar la incorporada directiva `ng-include` para encapsular plantillas estáticas para diferentes tipos de campos de entrada

```
<ng-include src="'input'+myinput.type+'.html'"></ng-include>
```

Donde el fragmento incluido especifica el tipo de campo de entrada como una cadena estática.

> `TIP` Presta atención a los problemas de ámbitos cuando se utilice esta técnica como `ng-include` crea un nuevo ámbito.

### Elementos personalizados HTML y versiones antiguas de internet explorer  

Por último, necesitamos mencionar que elementos personalizados HTML y atributos no son bien soportado por la versión 8 de Internet Explorer y anteriores. Hay paso adiciones que debe ser emprendidos para tomar todas las ventajas de las directivas AngularJS EN IE8 y IE7, y aquellas son describidas en detalles en el **Capítulo 12, Empaquetando y Desplegando Aplicaciones Web AngularJS**.

## Manejando la transformación del modelo con filtros.

Las expresiones AngularJS pueden llegar a ser bastante complejas y contener invocaciones de funciones. Esas funciones pueden servir diferentes propósitos pero las transformaciones y formateo del modelo son necesidades comunes. Para atender esos usos de caso comunes, AngularJS soporta funciones de formato especial (transformación) llamadas filtros.

```
{{user.signedUp | date:'yyyy-MM-dd'}}
```

En esta ejemplo el filtro date es usado para formatear la fecha de inscripción del usuario.

Un filtro es nada más que una función global que es invocada en la vista usando el símbolo tubo (`|`) con parámetros separados por el carácter dos puntos (`:`). En efecto podemos escribir nuestro código de ejemplo de la siguiente forma (Siempre que la función formatDate es definida en el ámbito):

```
{{ formatDate( user.signedUp, 'yyyy-MM-dd')}}
```

Ventajas de los filtros son de dos tipos: no requieren el registro de funciones en un ámbito (así son fácilmente accesibles para todas las plantilla), y usualmente ofrecen una más conveniente sintaxis comparada con la regulares llamadas a funciones.

Este simple ejemplo también muestra que los filtros se pueden parametrizar (o en otras palabras, se pueden pasar argumentos a una función filtro): Aquí donde especificamos un formato de fecha como un argumento a el filtro date.

Muchos filtros pueden ser combinados (encadenados) para formar una tubería de transformaciones. Como un ejemplo podemos formatear una cadena limitandola a 80 caracteres y convertir todos en caracteres en minúsculas.     

```
{{myLongString | limitTo:80 | lowercase}}
```

### Trabajando con filtros incorporados

AngularJS viene con varios filtros incluidos como parte de la librería principal. En general podemos dividir los filtros incorporados en dos grupos: filtros de formato y

### Filtros de formato

Aqui una lista de filtros de formato incorporados. Su propósito y escenarios de uso son fáciles de descifrar:  

- `currency`: Da formato a números con dos decimales y un símbolo de moneda.
- `date`: Da formato a la fecha acorde con un especifico formato de fecha. Los modelos puede contener fechas expresadas como objetos Fecha o como cadenas (en este caso las Cadenas serán parseadas a un objeto Fecha antes de formatear).
- `number`: Da formato un campo de entrada con un número de decimales especificado como un argumento a este filtro.
- `lowercase` y `uppercase`: Como implica lo mismo, estos filtros pueden ser usados para dar formato a cadenas a su forma lowercase ó uppercase.
- `json`: Este filtro es útil sobre todo para propósitos de depuración ya que puede garantizar "pretty-print" para objetos JavaScript. El uso típico se parece a lo siguiente: `<pre>{{someObject | json}}</pre>`. Es mayormente usado para  propósitos de depuración.

### Filtros de transformación de Arrays o Matrices

Por defecto AngularJS provee tres filtros que operan con matrices:

- `limitTo`: Devuelve un array reducido al el tamaño especificado. Podemos retener elementos de cualquier lugar, del principio de una colección o desde su final (en esta caso el parámetro límite, debe ser provisto como un número negativo).

- `Filter`: Esta es una utilidad de filtrado de propósito general. Es muy flexible y soporta muchas opciones para con precisión seleccionar elementos de una colección.

- `orderBy`: Filtros de orden pueden ser usados para organizar elementos individuales en un array basados en un criterio previsto.  
 
> `TIP` Los filtros listados trabajan solamente en arrays (`limiTo` es la excepción, puede hacerle  frente a las cadenas también). Cuando apliquemos otro tipo de objeto, esos filtros no tendrán efecto y simplemente retornarán un objeto fuente.  

Los filtros relacionados a los arrays son a menudo usados con la directiva `ng-repeat` para representar resultados. En las siguientes secciones vamos a construir un ejemplo completo de un tabla que puede ser organizada, filtrada y paginada. Los ejemplos están construidos alrededor del listado de atraso SCRUM de la aplicación de ejemplo, y ilustraremos cómo combinar los filtros y las directivas repetidoras.   

### Filtrado con el filtro “filter”

Primeramente necesitamos clarificar que AngularJS tiene un filtro llamado `filter`. El nombre es un poco inoportuno ya que la palabra “filter” puede referirse a cualquier filtro en general (una función transformadora) o este filtro específico llamado “filter”.

El *filtro* “*filter*” es una función de filtrado de propósito general que puede ser usada para seleccionar un subconjunto de un array (o dicho de otra manera excluir algunos elementos). Hay una serie de formatos de parámetros que pueden ser suministrado a este filtro con el fin de conducir el proceso de selección de elementos. En la forma más simple podemos ofrecer una cadena en el caso de que todos los campos de todos los elementos en una colección serán comprobados para una tal presencia de una subcadena dada.   

Como un ejemplo vamos a considerar un producto de la lista de atrasos que nos gustaría filtrar en base a criterios de búsqueda. A los usuarios se les presentará un cuadro de entrada donde podrán escribir los criterios de búsqueda. El resultado de la lista debe tener sólo elementos donde cualquier campo para un elemento dado contenga una subcadena proporcionada. La siguiente impresión de pantalla ilustra una Interfaz de usuario terminada.  

![chapter-4-3](/img/chapter-4-3.png)

Si asumimos que nuestro modelo de datos tiene las siguientes propiedades: `name`, `desc`, `priority`, `estimation` y `done`, podemos escribir una plantilla para la interfaz de usuario discutida de la siguiente manera:

```
<div class="well">
   <label>
       Search for:<input type="text" ng-model="criteria">
   </label>
</div>
<table class="table table-bordered">
   <thead>
   <th>Name</th>
   <th>Description</th>
   ...
   </thead>
   <tbody>
   <tr ng-repeat="backlogItem in backlog | filter:criteria">
       <td>{{backlogItem.name}}</td>
       <td>{{backlogItem.desc}}</td>
       ...
   </tr>
   </tbody>
</table>
```

Como puedes ver se ve extremadamente fácil añadir un filtro basado en la entrada del usuario; solo necesitamos escribir un valor de un campo de entrada como un argumento para el filtro. El resto será atendido por AngularJS automáticamente, el entrelazado de datos y el mecanismo de refrescar.

> `TIP` El criterio coincidente puede ser negado por el operador ! prefijado.

En el ejemplo previo todas las propiedades de los objetos fuentes son buscadas para una coincidencia de subcadena. Si nosotros queremos tener un control más preciso sobre las propiedades que coincidan lo podemos hacer proporcionando un argumento objeto a un filtro. Tal objeto actuará como “consulta de ejemplo”. Aquí queremos limitar las coincidencias a el nombre de la propiedad y y incluir solo los articulos que no se hayan hecho:

```
ng-repeat="item in backlog | filter:{name: criteria, done: false}"
```

En este fragmento de código todas las propiedades de un objeto especificado como un argumento deben coincidir. Podemos decir que las condiciones expresadas por las propiedades individuales son combinadas usando el operador lógico AND.
  
AngularJS adicionalmente provee un nombre de propiedad que encuentra todo: `$`. El uso de este comodín como un nombre de propiedad podemos combinar los operadores de lógica AND y OR. Digamos que queremos buscar una cadena en todas las propiedades de un objeto fuente, pero teniendo en cuenta que sólo en los artículos no completados. En este caso una expresión de filtrado puede ser reescrita de la siguiente manera:

```
ng-repeat="item in backlog | filter:{$: criteria, done: false}"
```

Puede ocurrir que la combinación del requerido criterio de búsqueda sea tan complejo que no sea posible expresarla usando la sintaxis de objetos. En este caso una función puede ser proporcionada como un argumento a el filtro (llamada función `predicate`). Tal función será invocada para todos y cada uno de los elementos individuales de una colección fuente. El array resultante contendrá sólo elementos para los cuales las función filtrante retorna `true`. Como ejemplo un poco artificioso podemos imaginar que queremos ver solo artículos atrasados que ya se han completado y requieren más de 20 unidades de esfuerzo. La función de filtrado para este ejemplo es a la vez fácil de escribir:

```
$scope.doneAndBigEffort = function (backlogItem) {
   return backlogItem.done && backlogItem.estimation > 20;
};
```

Y usa:

```
ng-repeat="item in backlog | filter:doneAndBigEffort"
```

### Contando resultados filtrados

A menudo, en ocasiones, nos gustaría desplegar un número de artículos en una colección. Normalmente es tan simple como usar la expresión: ```{{myArray.length}}```. Las cosas se ponen un poco más complicadas durante el uso de filtros, como nos gustaría mostrar el tamaño de una matriz filtrada. Un enfoque ingenuo podría consistir en duplicar filtros en ambos en un repetidor y en una expresión contadora. Tomando el último ejemplo de filtración en un repetidor:

```
<tr ng-repeat="item in backlog | filter:{$: criteria, done: false}">
```

Podemos tratar de crear una fila de resumen como:

```
Total: {{(backlog | filter:{$: criteria, done: false}).length}}
```

Esto tiene obviamente varios inconvenientes, no sólo se duplica el código, sino también los mismos filtros necesitan ser ejecutados varias veces en dos lugares diferentes, no es ideal desde el punto de vista del rendimiento.

Para remediar esta situación podemos crear una variable intermediaria (`filteredBacklog`) que mantendrá una matriz filtrada:
  
```
ng-repeat="item in filteredBacklog = (backlog | filter:{$: criteria, done: false})"
```

Entonces, contar los resultados de búsqueda se reduce a la visualización de la longitud de una matriz guardada.

```
Total: {{filteredBacklog.length}}
```

El precedente patrón para contar objetos filtrados, aunque no es muy intuitivo, nos permite disponer de una lógica de filtrado en un solo lugar.  

La otra posibilidad es mover toda la lógica de filtrado a el controlador y sólo exponer los resultados filtrados en el ámbito. Este método tiene tiene una ventaja más: mueve el código de filtrado a un controlador donde es muy fácil hacer pruebas unitarias. Para usar esta solución tu necesitaras aprender como acceder a los filtros desde JavaScript; algo que se cubre más adelante en este capítulo.

### Ordenando con el filtro orderBy

Muy a menudo unos datos tabulados pueden ser organizados libremente por los usuarios. Por lo general, al hacer clic en un encabezado de una columna individual, se selecciona un campo determinado como criterio de clasificación, mientras hace clic de nuevo invierte el orden de clasificación. En esta sección, vamos a implementar este patrón común con AngularJS.

El filtro `orderBy` será nuestra principal herramienta para este trabajo, Cuando haya terminado, nuestra tabla de ejemplo tendrá una lista de artículos atrasados que obtendrá iconos de organización totalmente funcionales, como se muestra a continuación:   


![chapter-4-4](/img/chapter-4-4.png)


El filtro `orderBy` es sencillo e intuitivo de usar por lo que podemos sumergirnos inmediatamente en el ejemplo de código, sin tener que gastar demasiado tiempo en introducciones teóricas. En primer lugar vamos a hacer el trabajo de clasificación y luego agregar los indicadores de clasificación.

```
<thead>
<th ng-click="sort('name')">Name</th>
<th ng-click="sort('desc')">Description</th>
. . .
</thead>
<tbody>
<tr ng-repeat="item in filteredBacklog = (backlog |
filter:criteria | orderBy:sortField:reverse)">
   <td>{{item.name}}</td>
   <td>{{item.desc}}</td>
   ...
</tr>
</tbody>
```

La actual clasificación es atendida por el filtro `orderBy`. Que en nuestro ejemplo toma dos argumentos:

- `sortField`: una propiedad llamada para ser usada como un predicado de clasificación.
- `sort` orden de clasificación (reversa): este argumento indica si una matriz debería invertirse.

La función sort, desencadenada por un evento click en la celda de cabecera, es responsable por seleccionar el campo de clasificación como también alternar la dirección de clasificación.  He aquí los relevante bits de código del controlador:   

```
$scope.sortField = undefined;
$scope.reverse = false;

$scope.sort = function (fieldName) {
  if ($scope.sortField === fieldName) {
    $scope.reverse = !$scope.reverse;
  } else {
    $scope.sortField = fieldName;
    $scope.reverse = false;
  }
};
```

Nuestro ejemplo de clasificación construye sobre la implementación previa, la filtrada, por lo que ahora nuestra lista de atrasos puede ser ambas, filtrada y clasificada. Con AngularJS es sorprendentemente fácil combinar ambos filtros para crear tablas interactivas.  

> `TIP` El filtro `orderBy` fue deliberadamente colocado luego del filtro `filter`. La razón para esto es rendimiento: la clasificación es más consistente en comparación a el filtrado, por lo que es mejor ejecutar el algoritmo de clasificación en un conjunto de datos mínimos.      

Ahora que la clasificación funciona solo necesitamos añadir iconos indicando cual campo estamos clasificando y si se trata de ascendente o descendente. Una vez más la directiva `ng-class` probara ser muy útil. He aquí un ejemplo de indicadores visuales para la columna “`name`”:

```
<th ng-click="sort('name')">Name
   <i ng-class="{'icon-chevron-up': isSortUp('name'), 'icon-chevron-down': isSortDown('name')}"></i>
</th>
```

Las funciones `isSortUp` y `isSortDown` son muy simples y luce de la siguiente forma:

```
$scope.isSortUp = function (fieldName) {
  return $scope.sortField === fieldName && !$scope.reverse;
};
$scope.isSortDown = function (fieldName) {
  return $scope.sortField === fieldName && $scope.reverse;
};
```

Por supuesto que hay muchas maneras de mostrar indicadores de clasificación, y la que acabamos de presentar se esfuerza por mantener clases CSS fuera del código JavaScript. Esta forma de presentación puede ser fácilmente cambiada sólo tienes que cambiar o retocar la plantilla.

### Escribiendo filtros personalizado - un ejemplo de paginación

Hasta ahora nos las hemos arreglado para mostrar los elementos de trabajo pendiente en una tabla dinámica que soporta calcificado y ordenado. La paginación es otro patrón de interfaz de usuario que es usado con un largo conjunto de datos.

AngularJS no provee ningún filtro que nos ayude a elegir precisamente un subconjunto de una matriz sobre la base de los índices de inicio y fin. Para soportar paginación necesitamos crear un nuevo filtro, y esta es una buena ocasión para familiarizarse con el proceso de escribir filtros personalizado.

Para conseguir la idea de una interfaz para un nuevo filtro, llamémosle `pagination` escribiremos un esbozo del lenguaje de marcado primeramente.

```
<tr ng-repeat="item in filteredBacklog = (backlog |
pagination:pageNo:pageSize">
   <td>{{item.name}}</td>
   . . .
</tr>
```

El nuevo filtro de `pagination` necesita tomar dos parámetros: pagina a ser desplegada (su indice) y su tamaño (número de elementos o artículos por página).

Lo que sigue es la primera, nativa implementación del filtro (el manejo de errores fue deliberadamente omitido para enfocarnos en la mecánica de la escritura del filtro):

```
angular.module('arrayFilters', [])
  .filter('pagination', function(){
    return function(inputArray, selectedPage, pageSize) {
      var start = selectedPage*pageSize;
      return inputArray.slice(start, start + pageSize);
    };
  });
```

Un filtro, como cualquier otro proveedor, necesita ser registrado en una instancia de un módulo. El método `filter` debe ser llamado con un nombre de filtro y una función de  fábrica que va a crear una instancia de un nuevo filtro. La función fábrica registrada debe retornar la actual función filtro.  

El primer argumento de la función de filtración `pagination` representa un campo de entrada a ser filtrado mientras que los parámetros subsiguientes pueden ser declarado para soportar opciones del filtro.

Los filtros son fáciles de probar con pruebas unitarias;  ellas trabajan en un campo de entrada suministrado, y cuando termina correctamente no deberían tener ningún efecto secundario. Aqui un ejemplo para probar nuestro filtro de paginación personalizado:

```
describe('pagination filter', function () {

  var paginationFilter;

  beforeEach(module('arrayFilters'));

  beforeEach(inject(function (_paginationFilter_) {
    paginationFilter = _paginationFilter_;
  }));

  it('should return a slice of the input array', function () {
    var input = [1, 2, 3, 4, 5, 6];
    expect(paginationFilter(input, 0, 2)).toEqual([1, 2]);
    expect(paginationFilter(input, 2, 2)).toEqual([5, 6]);
  });

  it('should return empty array for out-of bounds', function () {
    var input = [1, 2];
    expect(paginationFilter(input, 2, 2)).toEqual([]);
  });

});
```
 
Probar filtros es simple como probar una sola función, y la mayoría del tiempo es realmente sencillo. La estructura de la prueba de ejemplo justamente presentada debería ser fácil de seguir como casi no hay nuevas construcciones aquí, la única cosa que requiere explicación es la forma de acceder a instancias de un filtro desde el código JavaScript.

### Acceso a filtros desde código JavaScript

Los filtros son usualmente invocados desde el lenguaje de marcado (utilizando el símbolo de canalización “|” en las expresiones), pero es también posible obtener acceso a las instancias de los filtros desde el código JavaScript (controladores, servicios, otros filtros, etc). Des esta forma podemos combinar los existentes filtros para proveer una nueva funcionalidad.

Los filtros pueden ser inyectados a los objetos administrados por el **sistema de inyección de dependencia AngularJS**. Podemos expresar dependencia en un filtro usando dos distintos métodos, que requiera ya sea:

- El servicio `$filter`.
- Un nombre de filtro con el sufijo `Filter`.

El servicio `$filter` es una función de búsqueda que nos permite recuperar una instancia de un filtro basado en su nombre. Para verlo en acción, podemos escribir un filtro que se comporte de manera similar al filtro `limitTo` y pueda cortar el largo de las cadenas. Adicionalmente nuestra versión personalizada añadirá el sufijo “...” si una cadena es recortada. He aquí el código relevante:

```
angular.module('trimFilter', [])
  .filter('trim', function($filter){
    var limitToFilter = $filter('limitTo');
    return function(input, limit) {
      if (input.length > limit) {
        return limitToFilter(input, limit-3) + '...';
      }
      return input;
    };
  });
```

La llamada a la función `$filter(‘limitTo’)` nos permite tener a la mano una instancia de filtro basada en el nombre del filtro.

Mientras el método previo ciertamente funciona hay una alternativa que es a menudo más rápida de escribir y fácil de leer:

```
.filter('trim', function(limitToFilter){
 return function(input, limit) {
   if (input.length > limit) {
     return limitToFilter(input, limit-3) + '...';
   }
   return input;
 };
});
```

En el segundo ejemplo, presentado aquí, es suficiente para declarar una dependencia llamada como `[filter name]Filter` donde el `[filter name]` es un nombre del filtro que queremos  recuperar.

> `TIP` Acceder a instancias de filtros usando el servicio $filter resulta en una vieja sintaxis, y es por eso que la encontramos la forma usando el sufijo Filter fácil de trabajar. Las únicas ocasiones en que el servicio $filter podria ser mas conveniente es cuando necesitamos obtener varias instancias en un lugar o obtener una instancia de filtro basado en una variable, por ejemplo, $filter(filterName).

### Filtros, Hacer y qué evitar.

Los filtros hacen un maravilloso trabajo cuando son usados para formatear y transformar datos, invocados desde una plantilla ofreciendo una sintaxis agradable y concisa.  Pero los filtros son justamente herramientas para un específico trabajo y como cualquier otra herramienta puede causar daño si es usada incorrectamente. Esta sección describe situaciones donde los filtros deberían ser evitados y una solución alternativa debería ser una mejor solución.

### Filtros y manipulación DOM.  

A veces puede ser tentador retornar lenguaje de marcado HTML como resultado de la ejecución de filtros. En efecto AngularJS contiene un filtro que hace exactamente eso: linky (en el módulo separado `ngSanitize`).

Resulta que, en la práctica, los filtros retornado HTML no son la mejor idea. El problema principal es que para representar una salida de tal filtro necesitamos usar una de las directivas de entrelazado de datos descrita anteriormente en `ngBindUnsafeHtml` o `ngBindHtml`.  No solo esto hace que la sintaxis de entrelazado de datos sea más verbosa (comparado con la simple implementación `{{expression}}` ) y potencialmente hace una web vulnerable a un ataque de injection HTML.

Para ver algunos problemas relacionados con que los filtros emitan HTML, podemos examinar un simple filtro para destacar (`highlight`):    

```
angular.module('highlight', [])
  .filter('highlight', function(){
    return function(input, search) {
      if (search) {
        return input.replace(new RegExp(search, 'gi'),'<strong>$&</strong>');
      } else {
        return input;
      }
    };
  });
```

Puedes inmediatamente ver que este filtro contiene lenguaje de marcado HTML no modificable o hardcoded (es más difícil de cambiar si más adelante se hace necesario). Como resultado no podemos usarla con la directiva de interpolación, pero es necesaria para escribir una plantilla como:

```
<input ng-model="search">
<span ng-bind-html="phrase | highlight:search"></span>
```

Además de esto el lenguaje de marcado HTML emitido por el filtro no puede contener  directivas AngularJS ya que no serán evaluadas.

Una directiva personalizada, la mayoría del tiempo, resolverá el mismo problema en una forma más elegante, sin introducir potenciale riesgos de seguridad. La directivas son cubiertas en el **Capítulo 9, Construyendo Directivas Avanzadas y Capítulo 10, Construyendo Aplicaciones Web AngularJS para una Audiencia Internacional**.

### Costosa transformación de datos en filtros

Los Filtros, cuando son usados en una plantilla, se convierten en una parte integral de la expresión AngularJS, y como tal son frecuentemente evaluados. De hecho, tales funciones de filtrado son llamadas múltiples veces en cada ciclo de asimilación (digest). Podemos ver fácilmente esto en la práctica creando un envoltorio de registro alrededor del filtro mayúsculas (uppercase):

```
angular.module('filtersPerf', [])
 .filter('logUppercase', function(uppercaseFilter){
   return function(input) {
     console.log('Calling uppercase on: '+input);
     return uppercaseFilter(input);
   };
 });
```

Al usar este filtro recién definido en un lenguaje de mercado tal como:

```
<input ng-model="name"> {{name | logUppercase}}
```

Veremos que la declaración de registro se escribe al menos una vez (usualmente dos veces) por cada golpe de teclado! Este experimento solamente debería convencerte que los filtros son ejecutados a menudo por lo que es altamente preferible que se ejecuten rápido.

`TIP`: No se sorprenda de ver que los filtros son llamados varias veces en un fila. Esta es la comprobación sucia de AngularJS en funcionamiento. Esfuércese por escribir sus filtros simples, para que ejecuten un procesamiento rápido y ligero.  

### Filtros inestables

Ya que los filtros son llamados múltiples veces es razonable esperar que un filtro responda con el mismo valor retornado si el campo de entrada no ha cambiado. Tales funciones son llamadas estables con respecto a sus parámetros.

Las cosas se pueden fácilmente salirse de las manos si un filtro no tiene esta propiedad.  Para ver el efecto desastroso de los filtros inestables vamos a  escribir un malicioso filtro `random` que selecciona un elemento aleatorio de una matriz de entrada (es inestable):      

```
angular.module('filtersStability', [])
 .filter('random', function () {
   return function (inputArray) {
     var idx = Math.floor(Math.random() * inputArray.length);
     return inputArray[idx];
   };
 })
```

Dado una matriz de diferentes elementos almacenados en la variable `items` en un ámbito, el filtro `random` puede ser usado en una plantilla tal como:

```
{{items | random}}
```

El código precedente, después de la ejecución, imprimirá un valor aleatorio por lo que puede parecer que se comporta correctamente. Es sólo al inspeccionar la consola del navegador, nos damos cuenta que en efecto, un error es registrado:

```
Uncaught Error: 10 $digest() iterations reached. Aborting!
```

Este error significa que una expresión está dando diferentes resultados cada vez que sea siendo evaluada. AngularJS ve un constante cambio del modelo y reevalúa la expresión, con la esperanza de que se estabilizara. Luego repetir este proceso diez veces la digestión (digest) es abortada, el último resultado es impreso y el error archivado en la consola. **Capítulo 11, Escribiendo Robustas Aplicaciones Web** se entra en una discusión más profunda de estos temas, y explica el funcionamiento interno de AngularJS que debería hacer razones para que este error sea fácil de entender.  

En situaciones como esta la solución es calcular un valor aleatorio en un controlador, antes de que la plantilla sea representada.  

```
.controller('RandomCtrl', function ($scope) {
  $scope.items = new Array(1000);
  for (var i=0; i<$scope.items.length; i++) {
    $scope.items[i] = i;
  }
  $scope.randomValue = Math.floor(Math.random() * $scope.items.length);
});
```

Como esta, un valor aleatorio será calculado antes que se procese la plantilla y podamos usar de forma segura la expresión `{{randomValue}}` para emitir el valor preparado.

Si su función puede generar diferentes resultados para el mismo campo de entrada no es un buen candidato para un filtro. En su lugar invoque esta función desde un controlador y deje a AngularJS que reprecente el valor pre-calculado.

## Resumen

En este capítulo se nos llevó a través de un conjunto de patrones utilizados para mostrar los datos que están contenidos en el modelo.

Empezamos rápidamente cubriendo las convenciones de nombre de las directivas y luego nos trasladamos a la visión general de las directivas AngularJS incorporadas. La directiva `ng-repeat` tiene especial atención, ya que es muy poderosa y es una de las directivas de uso más frecuente.

Mientras que las plantillas declarativas, basadas en DOM, en la que AngularJS se  basa, trabaja perfectamente la mayoría del tiempo, hay algunos casos puntuales donde nosotros tocamos los límites de este enfoque. Es importante reconocer estas situaciones y prepararse para cambiar ligeramente objetivo del lenguaje de marcado cuando sea necesario.  

Los filtros ofrecen una sintaxis muy conveniente para formatear el modelo de específicas interfaces de usuario. Nosotros vimos que AngularJS provee muchos filtros útiles por defecto, pero cuando uno específico es necesitado surge que es muy fácil de crear un filtro personalizado. Que de las funciones de filtrado no se debe abusar y tuvimos una mirada cuidadosa a los escenarios donde un constructor alternativo debería ser considerado.

Las directivas, filtros y patrones de representación cubiertos en este capítulo están enfocados en la representación de datos. Pero antes que los datos puedan ser representados por los usuarios necesitamos ser capaces de ingresarlos usando una variedad de elementos de entrada. En el capítulo siguiente se sumerge en la forma en que AngularJS trata con elementos de formularios y campos de entrada problemáticos.