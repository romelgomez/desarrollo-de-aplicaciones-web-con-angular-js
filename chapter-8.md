## Capítulo 8 - Construye tus propias directivas

Si bien puedes llegar lejos usando sólo controladores y las directivas integradas que vienen con AngularJS, habrá inevitablemente un punto en el desarrollo de su aplicación donde necesitaras enseñar al navegador algunos nuevos trucos construyendo sus propias directivas. La razones principales del porqué le gustaría desarrollar directivas personalizadas son:  

- Necesitas manipular el DOM directamente, tal como con JQuery
- Quieres refactorizar partes de su aplicación para remover código repetidos.
- Quieres crear un nuevo marcado HTML para ser usado por los no desarrolladores, tal como diseñadores.  

Este capítulo le dará a conocer el desarrollo de su propia directiva AngularJS. Las directivas aparecen en muchos lugares y tienen diferentes usos. En este capítulo le mostraremos.

- Como definir una directiva
- Ejemplos de los tipos de directivas más comunes y cómo programarlas.
- Directivas que dependen unas de otras.
- Como realizar pruebas unitarias en directivas.

## ¿Qué son las directivas AngularJS?

La directivas son, sin duda, la característica más poderosa de AngularJS. Ella son el pegamento que une la lógica de su aplicación al DOM HTML. El siguiente diagrama ilustra como las directivas encajan dentro de la arquitectura de una aplicación AngularJS:

![chapter-8-1](/img/chapter-8-1.png)

Al extender y personalizar en como el navegador se comporta con respecto al HTML, las directivas permiten al desarrollador de la aplicación, o diseñador, enfocar su atención en que debe hacer la aplicación, o debe verse, en una manera declarativa, más que en la programación de las interacciones de bajo nivel con el DOM. Esto hace que el proceso de desarrollo sea más rápido, más fácil de mantener, y lo más importante más divertido!

Las directivas AngularJS añaden un nuevo significado y comportamiento al etiquetar en su aplicación HTML. Es dentro de la directivas, donde puede llegar a un bajo y sucio nivel con la manipulación del DOM, usualmente trabajando con jQuery o AngularJS jqLite.

> `Nota` Si cargas jQuery antes de AngularJS, entonces AngularJS usará jQuery para la manipulación del DOM. Por otra parte AngularJS asume que no estas usando jQuery y implementa su propia versión mínima e interna de jQuery, la cual es a menudo referida como jqLite.

El trabajo de la directiva es modificar la estructura del DOM y enlazar el ámbito a el DOM.  Esto significa manipular y enlazar nodos DOM basados en los datos en el ámbito, pero también enlazar eventos DOM para llamar a métodos en el ámbito.

### Comprendiendo las directivas integradas

AngularJS viene con un número de directivas integradas al marco. Están los obvios atributos y elementos HTML personalizados, tales como `ng-include`, `ng-controller` y `ng-click`. Estan tambien los que parecen ser elementos HTML estándar, tales como `script`, `a` , `select` , y `input`. Todas estas directivas son proporcionadas por el núcleo del marco AngularJS.

Lo importante es que las directivas integradas están definidas usando la misma API de directivas que podemos usar en nuestras aplicaciones. No hay nada especial sobre ellas en comparación con las directivas que los desarrolladores de aplicaciones pueden construir. Observando las directivas en el código fuente AngularJS pueden ser una excelente forma de aprender a desarrollar su propias directivas.

> `NOTA` Las directivas AngularJS integradas pueden ser encontradas en la carpeta `src/ng/directive` del proyecto AngularJS (`https://github.com/angular/angular.js/tree/master/src/ng/directive/`)

### Usar directivas en el marcado HTML

Las directivas pueden aparecer como elementos, atributos, comentarios HTML, o clases CSS. También, cualquier directiva puede ser identificada en el HTML en un número de formatos diferentes.

Estos son algunos ejemplos del uso de una directiva en el marcado HTML (no todas estas serían apropiadas según el uso de la directiva):

```
<my-directive></my-directive>
<input my-directive>
<!-- directive: my-directive-->
<input class="my-directive">
```

El nombre canónico, cuando se emplea para definir y referir una directiva en JavaScript. es la versión camelCased, por ejemplo, `myDirectiva`.  

## Siguiendo el ciclo de vida de compilación de la directiva

Cuando AngularJS compila una plantilla HTML este atraviesa el DOM suministrado por el navegador y trata de igualar cada elemento, atributo, comentario, y clases CSS contra su lista de directivas registradas. Cuando iguala una directiva, AngularJS llama función de compilación de directivas, el cual retorna una función enlazada. AngularJS colecciona todas estas funciones enlazadas.  

> `NOTA` La etapa de compilación se realiza antes de que el ámbito ha sido preparado, y no hay datos de ámbito disponibles en la función de compilación.  

Una vez que todas las directivas han sido compiladas, AngularJS crea el ámbito y enlaza cada directiva a el ámbito llamando cada una de las funciones enlazadas.

> `NOTA` En la etapa de enlace, el ámbito comienza a ser enlazado a la directiva, y la función de enlace puede entonces cablear enlaces entre el ámbito y el DOM.

La etapa de compilación es mayormente una optimización. Es posible hacer casi todo el trabajo en la etapa de la función de enlace (excepto por algunas cosas avanzadas como acceder a la función de transclusion). Si considerás el caso de una directiva repetida (dentro de `ng-repeat`), la función de compilación de la directiva es llamada solo una vez, pero la función enlazada es llamada en cada iteración del repetidor, cada vez que los datos cambian.

> `NOTA` Las funciones de transclusion son explicadas en detalle en el *Capítulo 9, Construyendo Directivas Avanzadas*.

La siguiente tabla muestra que funciones de compilación son llamadas cuando el compilador AngularJS iguala las directivas. Tu puedes ver que cada función de compilación es llamada solo una vez cuando cada directiva es usada en una plantilla.  

![chapter-8-2](/img/chapter-8-2.png)

La siguiente tabla muestra que funciones enlazadas son llamadas cuando la plantillas es convertida a el HTML definitivo. Puedes ver que la función enlazada para `myDir` es llamada en cada iteración del repetidor:

![chapter-8-3](/img/chapter-8-3.png)

Si tienes una funcionalidad compleja que no se basa en los datos en el ámbito, entonces debe aparecer en la función de compilación, de modo que sólo se llame una vez.

## Escribir pruebas unitarias para las directivas

Las directivas tienen acceso de bajo nivel a el DOM y pueden ser complejas. Esto las hace propensas a errores y difíciles de depurar. Por lo tanto, más que en otras áreas de su aplicación, es importante que la directivas tengan una amplia gama de pruebas.

Escribir pruebas unitarias para las directivas puede parecer intimidante al principio, pero AngularJS proporciona algunas buenas características para que sea lo menos doloroso posible y usted cosechará los beneficios cuando sus directivas sean fiables y de mantener.

> `NOTA` AngularJS tiene un amplio conjunto de pruebas para su directivas integradas. Ellas pueden ser encontradas en la carpeta test/ng/directive folder del proyecto AngularJS (https://github.com/angular/angular.js/tree/master/test/ng/directive/).

La estrategia general cuando probamos directivas es la siguiente:

- Cargar el módulo que contiene la directiva
- Compilar una cadena de marcado conteniendo la directiva para obtener la función enlazada
- Ejecutar la función enlazada para enlazarla a el `$rootScope`
- Verificar que el elemento tenga las propiedades que usted espera.

He aquí un esqueleto común de una prueba unitaria para una directiva:

```
describe('myDir directive', function () {
 var element, scope;
 beforeEach(module('myDirModule'));
 beforeEach(inject(function ($compile, $rootScope) {
   var linkingFn = $compile('<my-dir></my-dir>');
   scope = $rootScope;
   element = linkingFn(scope);
 }));
 it('has some properties', function() {
   expect(element.someMethod()).toBe(XXX);
 });
 it('does something to the scope', function() {
   expect(scope.someField).toBe(XXX);
 });
 ...
});
```

Cargar el módulo que contenga la directiva dentro de la prueba, luego crear un elemento que contenga esta directiva, usando las funciones `$compile` y `$rootScope`. Mantener una referencia del elemento y `$scope` de modo que esté disponible en todas las pruebas posteriores.

> `TIP` Dependiendo del tipo de prueba que estés escribiendo puedes que quieras compilar un diferente elemento en cada cláusula `it`. En esta caso debes mantener una referencia a la función `$compile` también.  

Finalmente, probar si la directiva se desempeña como se espera, interactuando con ella a través de las funciones de jQuery/jqLite y  a través de la modificación del ámbito.

En casos donde su directiva esté usando `$watch`, `$observe`, o `$q`, necesitaras desencadenar un `$digest` antes de comprobar su expectativas. Por ejemplo:

```
it("updates the scope via a $watch", function() {
 scope.someField = 'something';
 scope.$digest();
 expect(scope.someOtherField).toBe('something');
});
```

En el resto de este capítulo vamos a presentar nuestras directivas personalizadas a través de sus pruebas unitarias, de acuerdo con el concepto de **Test Driven Development (TDD)**.

## Definir una directiva

Cada directiva debe ser registrada con un módulo. LLamas la `directive()` en el módulo pasando el nombre canónico de la directiva y una función fabricante que retorna la definición de la directiva.

```
angular.module('app', []).directive('myDir', function() {
 return myDirectiveDefinition;
});
```

La función fabricante puede ser inyectada con servicios para ser utilizados por la directiva.

Una definición de directiva es un objeto cuyos campos indican al compilador lo que hace la directiva. Algunos de estos campos son declarativos (por ejemplo, `replace: true`, el cual indica al compilador reemplazar el elemento original con el que esta en la plantilla). Algunos campos son imperativos (por ejemplo, `link: function(...)`), los cuales proporcionan la función enlazada a el compilador.

Esta tabla describe todos los campos que pueden ser usados en una definición de directiva.


| Campo       | Descripción                                     |
| ------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name        | El nombre de la directiva.                                                                                                                                      |
| restrict    | En que tipo de marcado puede aparecer.                                                                                                                          |
| priority    | Sugiere al compilador el orden en que las directivas deben ser ejecutadas.                                                                                      |
| terminal    | Si el compilador debe continuar compilando directivas bajo este.                                                                                                |
| link        | La función de enlace que unirá la directiva a el ambito.                                                                                                        |
| template    | Una cadena que será usada para generar marcado para esta directiva.                                                                                             |
| templateUrl | Una URL donde se puede encontrar la plantilla para esta directiva.                                                                                              |
| replace     | Si se debe sustituir el elemento de esta directiva con lo que está en la plantilla.                                                                             |
| transclude  | Si se proporciona el contenido para este elemento-directiva para usarlo en la plantilla y la función de compilación                                             |
| scope       | Si se crea un nuevo ámbito hijo o un ámbito aislado para esta directiva.                                                                                        |
| controller  | Una función que actuará como controlador de directiva para esta directiva.                                                                                      |
| require     | Requiere un controlador de directiva de otra directiva para ser inyectado en esta función de enlace de esta directiva.                                          |
| compile     | La función de compilación que pueda manipular la fuente DOM y creará la función de enlace y sólo será usada si un link no ha sido proporcionado anteriormente.  |

La mayoría de las directivas que probablemente escribas sólo necesitarán algunos de estos campos. El resto de esta capítulo mostrará varias directivas personalizadas para nuestra aplicación SCRUM. Para cada directiva, miraremos las partes relevantes de la definición de la directiva.  

## Estilizar botones con directivas

Estamos usando los estilos CSS Bootstrap para nuestra aplicación. Esta librería CSS específica marcado y clases CSS para asegurar que los botones están estilizados correctamente. Por ejemplo un botón que tenga marcado de formulario.  

```
<button type="submit" class="btn btn-primary btn-large">Click Me!</button>
```

Tener que acordarse de aplicar estas clases es susceptible a errores y consume mucho tiempo. Podemos construir una directiva para hacer esto más sencillo y confiable.

### Escribiendo una directiva botón.

Todos los botones en nuestra aplicación deben ser estilizados como un botón CSS Bootstrap. En lugar que tener que recordar añadir `class="btn"` a cada botón, podemos añadir una directiva botón para haga esto por nosotros. La prueba unitaria para esto luce tal cual:

```
describe('button directive', function () {
 var $compile, $rootScope;
 beforeEach(module('directives.button'));
 beforeEach(inject(function(_$compile_, _$rootScope_) {
   $compile = _$compile_;
   $rootScope = _$rootScope_;
 }));
 it('adds a "btn" class to the button element', function() {
   var element = $compile('<button></button>')($rootScope);
   expect(element.hasClass('btn')).toBe(true);
 });
});
```

Cargamos el modelo, creamos un botón, y comprobamos que esta tenga la clase CSS correcta.

> `NOTA` Recuerda que el inyector ignora un par de guiones bajos (por ejemplo `_$compile_`) circundantes al nombre del parámetro. Esto nos permite copiar los servicios inyectados dentros de variables con el correcto nombre para su uso más tarde (por ejemplo, `$compile = _$compile_`)   

En adelante cualquier botón con `type="submit"` debe ser automáticamente estilizado como un botón primario y sería mejor establecer el tamaño del botón a través del un atributo de tamaño. Nos gustaría simplemente escribir:

```
<button type="submit" size="large">Submit</button>
```

Podemos crear la prueba unitaria de esta manera::

```
it('adds size classes correctly', function() {
 var element = $compile('<button size="large"></button>')
 ($rootScope);
 expect(element.hasClass('btn-large')).toBe(true);
});
it('adds primary class to submit buttons', function() {
 var element = $compile('<button type="submit"></button>')
 ($rootScope);
 expect(element.hasClass('btn-primary')).toBe(true);
});
```

Vamos a ver como esta directiva puede ser implementada:

```
myModule.directive('button', function() {
 return {
   restrict: 'E',
   compile: function(element, attributes) {
     element.addClass('btn');
     if ( attributes.type === 'submit' ) {
       element.addClass('btn-primary');
     }
     if ( attributes.size ) {
       element.addClass('btn-' + attributes.size);
     }
   }
 };
});
```

> `NOTA` Estamos asumiendo que tenemos un módulo `myModule` ya definido.

El nombre de nuestra directiva es un 'button' y esta restringida a aparecer sólo como un elemento ( restrict: 'E' ). Esto significa que esta directiva se aplicará siempre que el compilador AngularJS venga a través del elemento button. En efecto, estamos añadiendo comportamiento a el elemento boton HTML estándar.  

La única otra parte de esta directiva es la función de compilación. Esta función será llamada cada vez que el compilador coincida con esta directiva. La función de compilación se le pasa un parámetro llamado element. Este es un objeto jQuery (o jqLite) que referencia el elemento DOM donde la directiva aparece, en este caso, el elemento de botón en sí.

En nuestra función de compilación simplemente añadimos clases CSS al elemento basados en los valores de los atributos en el elemento. Usamos los parámetros de atributos inyectados para acceder a el valor de los atributos en el elemento.   

Podemos hacer todas estas modificaciones en la función de compilación en lugar de enlazar la función porque nuestros cambios para el elemento no se basan en datos del ámbito que se enlazaran al elemento. Podríamos haber puesto esta funcionalidad dentro de una función enlazada en su lugar, pero si el botón aparecen en un bucle `ng-repeat`, entonces `addClass()` sería  llamada para cada iteración del botón.

> `NOTA` Observa la sección Siguiendo el ciclo de vida de compilación de la directiva para una discusión detallada sobre esto.  
 
Colocando la funcionalidad en la función de compilación, esta es llamada solo una vez, y el botón es simplemente clonado por la directiva `ng-repeat`. Si estas haciendo un trabajo complejo en el DOM entonces esta optimización puede hacer una diferencia significativa, especialmente si está iterando sobre una colección grande.

## Entendiendo directivas widget AngularJS

Una de las más poderosas características de las directivas es poder crear tu propia etiquetas especificar del dominio.  En otras palabras, puedes crear elementos personalizados y atributos que den un nuevo significado y comportamiento a el marcado en su HTML que es específico a el dominio para el cual estás construyendo la aplicación.

Por ejemplo, similar a las etiquetas HTML estándar, puedes crear un elemento `<user>` para desplegar información del usuario o un elemento `<g-map>` que te permita interactuar con Google map. Las posibilidades son infinitas y el beneficio es que su marcado coincida con el dominio en el cual va a desarrollar.

### Escribiendo una directiva de paginación

En nuestra aplicación SCRUM, a menudo tenemos una larga lista de tareas o elementos de trabajo pendientes que son pesados de desplegar en la pantalla de una sola vez. Usamos paginación para romper la lista dentro de páginas manejables de elementos. Es común tener un bloque de control de paginación en nuestra pantalla que enlaza a las páginas en la lista.

El marco CSS Bootstrap proporciona un diseño simple para estilizar tal widget.

![chapter-8-4](/img/chapter-8-4.png)

Implementaremos este bloque de paginación como una directiva widget reusable que usaremos en nuestro marcado sin tener que pensar en los detalles de cómo funciona. El marcado se verá así:

```
<pagination num-pages="tasks.pageCount" current-page="tasks.currentPage"></pagination>
```

### Escribir pruebas para la directiva de paginación

Las pruebas para este widget necesitan cubrir todos los cambios que puedan ocurrir en la función `$scope` y desde que el usuario hace clic en los links. Esta es una selección de las pruebas más significativas:

```
describe('pagination directive', function () {
 var $scope, element, lis;
 beforeEach(module('directives'));
 beforeEach(inject(function($compile, $rootScope) {
   $scope = $rootScope;
   $scope.numPages = 5;
   $scope.currentPage = 3;
   element = $compile('<pagination num-pages="numPages" current-page="currentPage"></pagination>')($scope);
   $scope.$digest();
   lis = function() { return element.find('li'); };
   }));
   it('has the number of the page as text in each page item',
   function() {
     for(var i=1; i<=$scope.numPages;i++) {
     expect(lis().eq(i).text()).toEqual(''+i);
     }
     });
   it('sets the current-page to be active', function() {
     var currentPageItem = lis().eq($scope.currentPage);
     expect(currentPageItem.hasClass('active')).toBe(true);
     });
//    ...
   it('disables "next" if current-page is num-pages', function() {
     $scope.currentPage = 5;
     $scope.$digest();
     var nextPageItem = lis().eq(-1);
     expect(nextPageItem.hasClass('disabled')).toBe(true);
     });
   it('changes currentPage if a page link is clicked', function() {
     var page2 = lis().eq(2).find('a').eq(0);
     page2.click();
     $scope.$digest();expect($scope.currentPage).toBe(2);
     });
//    ...
   it('does not change the current page on "next" click if already at last page', function() {
     var next = lis().eq(-1).find('a');
     $scope.currentPage = 5;
     $scope.$digest();
     next.click();
     $scope.$digest();
     expect($scope.currentPage).toBe(5);
     });
   it('changes the number of items when numPages changes', function() {
     $scope.numPages = 8;
     $scope.$digest();
     expect(lis().length).toBe(10);
     expect(lis().eq(0).text()).toBe('Previous');
     expect(lis().eq(-1).text()).toBe('Next');
     });
   });
```

### Usando una plantilla HTML en una directiva

Este widget requiere que generemos algunas etiquetas HTML para reemplazar la directiva. La forma simple de hacer esto es utilizar una plantilla para la directiva. Aqui esta el marcado para esta plantilla.

```
<div class="pagination">
  <ul>
    <li ng-class="{disabled: noPrevious()}">
      <a ng-click="selectPrevious()">Previous</a>
    </li>
    <li ng-repeat="page in pages" ng-class="{active: isActive(page)}">
      <a ng-click="selectPage(page)">{{page}}</a>
    </li>
    <li ng-class="{disabled: noNext()}">
      <a ng-click="selectNext()">Next</a>
    </li>
  </ul>
</div>
```

La plantilla usa una matriz llamada pages y un número de funciones ayudantes, tales como `selectPage()` y `noNext()`. Estas son internas a la implementación del widget. Ellas necesitan ser colocadas en un ámbito para que la plantilla acceda a ellas pero no deben aparecer en el ámbito donde el widget es usado. Para lograr esto podemos solicitar al compilador crear un nuevo ámbito aislado para nuestra plantilla.

### Aislar nuestra directiva de su ámbito padre

No sabemos que contendrá el ámbito en el punto donde nuestra directiva es usada. Es una buena práctica, por consiguiente, proporcionar nuestra directiva con una interfaz de frente al público bien definida. Esto asegura que la directiva no se base o sea afectada por propiedades arbitrarias en el ámbito donde ésta es usada.

Tenemos tres opciones para el ámbito a ser usado en nuestra directiva y su plantilla.  Esta es definida en la definición de la directiva:

- Para rehusar el ámbito, desde el lugar donde el widget. Este es el por defecto y corresponde al `scope: false`.
- Para crear un ámbito hijo, que prototipicamente hereda del ámbito donde widget es usado.  Esto se especifica con `scope: true`.
- Para crear un ámbito aislado, el cual no herada prototipicamente, de manera que esté completamente aislado de su padre. Esto se especifica pasando un objeto a la propiedad: `scope: { ... }`

Queremos desacoplar completamente nuestro plantilla widget del resto de la aplicación, de manera que no haya peligro de filtración de datos entre los dos. Vamos a usar un ámbito aislado como se muestra en el siguiente código:

![chapter-8-5](/img/chapter-8-5.png)

> `NOTA` Mientras que un ámbito aislado no hereda prototipicamente de su padre, esta puede acceder a su ámbito padre a través de la propiedad `$parent`. Pero esto es considerado una mala práctica porque estás quebrantando el aislamiento de la directiva de su entorno.

Ya que nuestro ámbito está aislado del ambito padre, necesitamos explícitamente mapear los valores entre el ámbito padre y el ámbito aislado. Esto se hace haciendo referencia a expresiones AngularJS en los atributos del elemento donde la directiva aparece. En el caso de nuestra directiva de paginación, los atributos `num-pages` y `current-page` cumple este rol.

Podemos sincronizar las expresiones en estos atributos con propiedades en el ámbito de la plantilla a través de observadores. Podemos establecer esto manualmente o podemos pedirle a AngularJS que las conecte por nosotros. Hay tres tipos de interfaces que podemos especificar entre los atributos del elemento y el ámbito aislado:  interpolar (@) , enlace de datos (=) , y la expresión (&). Específicas estas interfaces como pares key-value en la propiedad ámbito de la definición de la directiva.

La llave es el nombre del campo en el ámbito aislado. El valor es uno de @, =, o & seguido del nombre del atributo en el elemento:

```
scope: {
  isolated1: '@attribute1',
  isolated2: '=attribute2',
  isolated3: '&attribute3'
}
```

Aquí tenemos definido tres campos en el ámbito aislado y AngularJS mapeará sus valores  a partir de los atributos especificados en el elemento donde las directivas aparecen.

> `NOTA` SI el nombre del atributo es omitido del valor, entonces este asume que el atributo tiene el mismo nombre que el campo del ámbito aislado: scope: { isolated1: '@' } Se espera que el atributo se llame isolated1.

### Interpolando el atributo con @

El símbolo `@` indica que AngularJS debe interpolar el valor del atributo especificado y actualizar la propiedad del ámbito aislado cuando este cambie. La interpolación es usada con las llaves `{{}}` para generar una cadena usando valores del ambito padre.

> `NOTA` Es un error común esperar que un objeto interpolado sea el objeto mismo. La interpolación siempre retorna una cadena. Así que si usted tiene un objeto, que dice que el user tiene un campo llamado userName, entonces la interpolación de {{user}} 	convertirá el objeto user en una cadena y no podrás acceder a la propiedad userName en la cadena.

Esta interpolación es equivalente a manualmente `$observe` el atributo:

```
attrs.$observe('attribute1', function(value) {
 isolatedScope.isolated1 = value;
});
attrs.$$observers['attribute1'].$$scope = parentScope;
```

### Enlazando datos al el atributo con =

El símbolo `=` indica que AngularJS debe mantener la expresión en el atributo especificado y el valor en el ámbito aislado sincronizado uno con el otro. Esto es un enlace de datos bidireccional que permite a los objeto y valores ser mapeados directamentes entre el interior y el exterior del widget.

> `NOTA` Ya que esta interfaz soporta enlace de datos bidireccional, la expresión dada en el atributo debe ser asignable (es decir, se refiere a un campo en el ámbito o a un objeto) y no una expresión computada arbitraria.  

Este enlace es tal como establecer dos funciones `$watch`:

```
var parentGet = $parse(attrs['attribute2']);
var parentSet = parentGet.assign;
parentScope.$watch(parentGet, function(value) {
 isolatedScope.isolated2 = value;
});
isolatedScope.$watch('isolated2', function(value) {
 parentSet(parentScope, value);
});
```

La implementación actual es más compleja para asegurar la estabilidad entre dos ámbitos.

### Proporcionando una expresión de retrollamada en el atributo con &

El símbolo & indica que la expresiones proporcionadas en el atributo en el elemento se pondrán a disposición en el ámbito como una función que, cuando es llamada, ejecutará la expresión. Esto es útil para crear retrollamadas desde el widget.  

Este enlace es equivalente a analizar (`$parse`) la expresión en el atributo y exponer la función de la expresión analizada en el ámbito aislado:

```
parentGet = $parse(attrs['attribute3']);
scope.isolated3 = function(locals) {
 return parentGet(parentScope, locals);
};
```

### Implementando el widget

Lo siguientes es la definición del objeto de la directiva de paginación:

```
myModule.directive('pagination', function() {
 return {
   restrict: 'E',
   scope: {
     numPages: '=',
     currentPage: '='
   },
   template: ...,
   replace: true,
```

La directiva es restringida a aparecer como un elemento. Esta crea un ámbito aislado con los datos `numPages` y `currentPage`, lo cuales se unen a los atributos `num-pages` y `current-page`, respectivamente. El elemento-directiva será reemplazado con la plantilla mostrada anteriormente:

```
link: function(scope) {
 scope.$watch('numPages', function(value) {
   scope.pages = [];
   for(var i=1;i<=value;i++) { scope.pages.push(i); }
   if ( scope.currentPage > value ) {
     scope.selectPage(value);
   }
 });
//...
 scope.isActive = function(page) {
   return scope.currentPage === page;
 };
 scope.selectPage = function(page) {
   if ( ! scope.isActive(page) ) {
     scope.currentPage = page;
   }
 };
//...
 scope.selectNext = function() {
   if ( !scope.noNext() ) {
     scope.selectPage(scope.currentPage+1);
   }
 };
}
```

La función link establece una propiedad `$watch` para crear la matriz de páginas basada en el valor de `numPages`. Esta añade varias funciones ayudantes en el ámbito aislado que será usado en la plantilla de la directiva.

### Añadiendo una retrollamada selectPage en la directiva

Sería útil proporcionar una función o una expresión que sea evaluada cuando la página cambia. Podemos hacer esto especificando un nuevo atributo en la directiva y asignarla al ámbito aislado utilizando & .

```
<pagination
  num-pages="tasks.pageCount"
  current-page="tasks.currentPage"
  on-select-page="selectPage(page)">
</pagination>
```

Lo que estamos diciendo aquí es que siempre que la página seleccionada cambie, la directiva debe llamar la funcion selectPage(page) pasandole el nuevo número de página como un parámetro. Aqui esta una prueba para esta característica:

it('executes the onSelectPage expression when the current page changes', inject(function($compile, $rootScope) {
  $rootScope.selectPageHandler =  jasmine.createSpy('selectPageHandler');
  element = $compile(
    '<pagination num-pages="numPages" ' +
    ' current-page="currentPage" ' +
    ' on-select-page="selectPageHandler(page)">' +
    '</pagination>')($rootScope);
    
  $rootScope.$digest();
  var page2 = element.find('li').eq(2).find('a').eq(0);
  page2.click();
  $rootScope.$digest();
  expect($rootScope.selectPageHandler).toHaveBeenCalledWith(2);
}));

Nosotros creamos observador para controlar la retrollamada y cuando la función `it` es llamada cuando hacemos click en una nueva página.

Para implementar esto añadimos un campo extra para nuestra definición aislada   

```
scope: {
 ...,
 onSelectPage: '&'
},
```

Ahora una función `onSelectPage()` estara disponible en el ámbito aislado. Cuando se llame, esta ejecutará la expresión pasada a el atributo `on-select-page`. Ahora cambiamos la función `selectPage()` en el ámbito aislado para llamar `onSelectPage()`:

```
scope.selectPage = function(page) {
 if ( ! scope.isActive(page) ) {
   scope.currentPage = page;
   scope.onSelectPage({ page: page });
 }
};
```

> `NOTA` Nota que pasamos la variable de la página a la expresión en un mapa de variables. Estas variables son proporcionadas a la expresión unida cuando esta es ejecutada, como si estuvieran en el ámbito.

## Creando una directiva de validación personalizada

En nuestra aplicación SCRUM tenemos un Formulario de Edición de Usuario. En tal formulario nosotros requerimos proporcionar una clave. Ya que el campo clave es ocultado y el usuario no puede ver que esta escribiendo, es muy útil tener un campo de clave de confirmación.

Necesitamos comprobar que los campos clave y la clave confirmada son idénticos. Crearemos un directiva de validación personalizada que podamos aplicar a un elemento input que compruebe si el modelo del elemento input concuerda con el otro valor del modelo. En uso, este se verá tal como:    

```
<form name="passwordForm">
 <input type="password" name="password" ng-model="user.password">
 <input type="password" name="confirmPassword" ng-model="confirmPassword" validate-equals="user.password">
</form>
```

Esta directiva de validación de modelo personalizada debe integrarse con `ngModelController` para proporcionar una consistente experiencia de validación para el usuario.

Nosotros podemos exponer el `ngModelController` en el ámbito proporcionado un nombre para el formulario y un nombre para el elemento `input`. Esto nos permite acceder a la validación del modelo en el controlador. Nuestra directiva de validación establecerá el input `confirmPassword` para validar si este valor de modelo es el mismo que el modelo `user.password`.

### Requiriendo una directiva controladora

Las directivas de validación requieren acceso a el `ngModelController`, cual es la directiva controladora para la directiva `ng-model`. Nosotros especificamos esto en nuestra definición de directiva usando el campo `require`. Este campo toma una cadena o un array de cadenas. En cada cadena debe ser el nombre canónico de la directiva cuyo controlador requerimos.

Cuando la directiva requerida es encontrada, su directiva controladora es inyectada dentro de la función enlazada como el cuarto parámetro. Por ejemplo:

```
require: 'ngModel',
link: function(scope, element, attrs, ngModelController) { ... }
```

Si más de un controlador es requerido, entonces el cuarto parámetro será un array que contenga esos controladores en el mismo orden que ellos son requeridos.

> `NOTA` Si el actual elemento con contiene la directiva especificada, entonces el compilador lanzará un error. Esto puede ser una buena forma de asegurar que la otra directiva ha sido proporcionada.

### Haciendo el controlador opcional.

Puedes hacer el campo requerido del controlador opcional colocando un '?' al frente del nombre del la directiva, por ejemplo, `require: '?ngModel'`. Si la directiva no ha sido proporcionada, el cuarto parámetro será null. Si requieres mas de un controlador entonces el elemento correspondiente en la matriz de controladores será `null`.

### Buscando ancestros o padres para el controlador

Si la directiva, cuyo controlador usted necesita, puede aparecer en este o cualquier ancestro del actual elemento, puede entonces colocar '^' al frente del nombre de la directivA, por ejemplo, require: '^ngModel'. El compilador buscará entonces los elementos antecesores a partir del elemento contenedor de la directiva actual y retornará el primer controlador coincidente.

> `TIP` Puedes combinar los prefijos ancestro y opcional para tener un directiva opcional que pueda aparecer en un ancestro. Por ejemplo, require: '^?form' le permitirá encontrar el controlador para la directiva form, que es lo que la directiva `ng-model` hace para registrarse con el form si esta esta disponible.

### Trabajando con ngModelController

Una vez tenemos el `ngModelController` podemos usar su API para especificar la validez del elemento input. Este es un caso común para esta tipo de directiva y el patrón bastante sencillo. El `ngModelController` expone las siguientes funciones y propiedades que usaremos:


| Nombre                                     | Descripción                                                                                      |
| -------------------------------------------|--------------------------------------------------------------------------------------------------|
| $parsers                                   | Una serie de funciones que serán llamadas a su vez, cuando el valor del elemento input cambie.   |
| $formatters                                | Una serie de funciones que serán llamadas a su vez, cuando el valor del modelo cambie.           |
| $setValidity(validationErrorKey, isValid)  | Una función llamada para establecer si el modelo es válido para un tipo de error de validación.  |
| $valid                                     | Verdadero si no hay ningún error.                                                                |
| $error                                     | Un objeto que contiene información sobre cualquier error de validación en el modelo              |


Las funciones que van dentro de `$parsers` y `$formatters` toman un valor y retornarán un valor, por ejemplo, `function(value){ return value; }`, El valor que reciben es el valor retornado de previas funciones en la serie. Es dentro de esas funciones donde colocamos nuestras lógica de validación y llamamos `$setValidity()`.

### Escribiendo pruebas de directivas de validación personalizadas

El patrón para probar las directivas de validación es compilar un formulario que contiene un input que usa `ng-model` y nuestra directiva de validación. Por ejemplo:

```
<form name="testForm">
  <input name="testInput" ng-model="model.testValue" validate-equals="model.compareTo">
</form>
```

Esta directiva es un atributo en el elemento `input`. El valor del atributo es una expresión que debe evaluar el valor en el modelo. La directiva comparará este valor con el valor del `input`.

Especificamos el modelo unido a este input usando la directiva `ng-model`. Este creará ngModelController,  cual será expuesto en el ámbito como `$scope.testForm.testInput` y el valor del modelo en sí estará expuesto en el ámbito como `$scope.model.testValue`.

Nosotros entonces realizamos cambios al valor del input y al valor del modelo y comprobamos el `ngModelController` por cambios en `$valid` y `$error`.

En la configuración de la prueba mantenemos una referencia al modelo y al `ngModelController`.

```
describe('validateEquals directive', function() {
 var $scope, modelCtrl, modelValue;
 beforeEach(inject(function($compile, $rootScope) {
   ...
   modelValue = $scope.model = {};
   modelCtrl = $scope.testForm.testInput;
   ...
 }));
 ...
 describe('model value changes', function() {
   it('should be invalid if the model changes', function() {
     modelValue.testValue = 'different';
     $scope.$digest();
     expect(modelCtrl.$valid).toBeFalsy();
     expect(modelCtrl.$viewValue).toBe(undefined);
   });
   it('should be invalid if the reference model changes', function()
   {
     modelValue.compareTo = 'different';
     $scope.$digest();
     expect(modelCtrl.$valid).toBeFalsy();
     expect(modelCtrl.$viewValue).toBe(undefined);
   });
   it('should be valid if the modelValue changes to be the same as
   the reference', function() {
   modelValue.compareTo = 'different';
   $scope.$digest();
   expect(modelCtrl.$valid).toBeFalsy();
   modelValue.testValue = 'different';
   $scope.$digest();
   expect(modelCtrl.$valid).toBeTruthy();
   expect(modelCtrl.$viewValue).toBe('different');
 });
});
```

Aquí modificamos ambos ámbitos, el modelo del elemento input en sí (`modelValue.testValue`) y el modelo del valor con el cual se compara el input (`modelValue.compareTo`). Luego probamos la validación del input (`modelCtl`). Tenemos que llamar `$digest()` para asegurarnos que el input ha sido actualizado desde que el modelo cambia.  

```
describe('input value changes', function() {
   it('should be invalid if the input value changes', function() {
       modelCtrl.$setViewValue('different');
       expect(modelCtrl.$valid).toBeFalsy();
       expect(modelValue.testValue).toBe(undefined);
   });
   it('should be valid if the input value changes to be the same as the reference', function() {
       modelValue.compareTo = 'different';
       $scope.$digest();
       expect(modelCtrl.$valid).toBeFalsy();
       modelCtrl.$setViewValue('different');
       expect(modelCtrl.$viewValue).toBe('different');
       expect(modelCtrl.$valid).toBeTruthy();
   });
});
```

Aquí modificamos el valor del campo, llamando `$setViewValue()`, que es lo que sucede si el usuario escribe o pega dentro del input.

### Implementando una directiva de validación personalizada

Ahora que tenemos nuestras pruebas en su lugar, podemos implementar la funcionalidad de la directiva.

```
myModule.directive('validateEquals', function() {
   return {
       require: 'ngModel',
       link: function(scope, elm, attrs, ngModelCtrl) {
           function validateEqual(myValue) {
               var valid = (myValue === scope.$eval(attrs.validateEquals));
               ngModelCtrl.$setValidity('equal', valid);
               return valid ? myValue : undefined;
           }
           ngModelCtrl.$parsers.push(validateEqual);
           ngModelCtrl.$formatters.push(validateEqual);
           scope.$watch(attrs.validateEquals, function() {
               ngModelCtrl.$setViewValue(ngModelCtrl.$viewValue);
           });
       }
   };
});
```

Creamos una funcion llamada `validateEqual(value)`, la cual compara el valor pasado con el valor de la expresión. Introducimos esto dentro de la series de funciones `$parsers` y `$formatters`, para que la función de validación sea llamada cada vez que el modelo o la vista cambie.

En esta directiva tenemos también que tener en cuenta el modelo que estamos comparando contra el que cambia. Hacemos esto estableciendo observadores en la expresión, la cual estamos recuperando desde el parámetro `attrs` de la función enlazada. Cuando esta cambia, nosotros artificialmente desencadenamos la serie de funciones `$parsers` para ejecutar llamando `$setViewValue()`. Esto asegura que todo el potencial `$parsers` es ejecutado en caso de que cualquiera de ellos modifique el valor antes de que llegue nuestro validador.

## Crear un validador de modelo asíncrono

Algunas validaciones pueden ser solo echas interactuando con un servicio remoto, por ejemplo una base de datos. En estos casos, la respuestas del servicio será asíncrona. Esto brinda complicaciones no sólo en el trabajo con la validación de modelos de forma asíncrona sino también en la prueba de esta funcionalidad.

En nuestro Formulario de usuario Administrativo (Admin User Form), nos gustaría comprobar si la dirección de correo que el usuario introdujo ya ha sido tomada. Crearemos una directiva `uniqueEmail` (correo único) la cual comprobará con nuestro servidor back-end para encontrar si la dirección de correo ya está en uso:

```
<input ng-model="user.email" unique-email>
```

### Simular el servicio User

Usamos el servicio de recursos de usuarios para solicitar a la base de datos las direcciones de correo que ya están en uso. Necesitamos simular el método `query()` en este servicio para nuestras pruebas

> `NOTA` En este caso, es más fácil crear un modulo de prueba y simular la salida del objeto del servicio `User` en lugar de inyectar el servicio y espiar el método `query()` ya que el servicio User confía en una serie de otros servicios y constantes.

```
angular.module('mock.Users', []).factory('Users', function() {
   var Users = { };
   Users.query = function(query, response) {
       Users.respondWith = function(emails) {
           response(emails);
           Users.respondWith = undefined;
       };
   };
   return Users;
});
```


La función query crea `User.respondWith()` que llamara la retrollamada response que será pasada a la `query()`. Esto nos permitirá simular una respuesta para la llamada en nuestras pruebas.

> `NOTA`: Antes `User.query` ha sido llamado y luego a sido manipulado con una llamada a `User.respondWith()`, nosotros establecemos la función `Users.respondWith` a sin definir.

Entonces cargamos este módulo, además de módulo bajo prueba:

```
beforeEach(module('mock.users'));
```

Esto causa que el servicio original Users sea reescrito por nuestro servicio simulado.

### Escribir pruebas para validación asíncrona

Establecemos pruebas similares a las anteriores directivas de validación:

```
beforeEach(inject(function($compile, $rootScope, _Users_){
   Users = _Users_;
   spyOn(Users, 'query').andCallThrough();
   ...
}));
```

Estamos observando la función `Users.query()` pero queremos también llamar a través de nuestros función de salida simulada para que podamos simular respuestas con `Users.respondWith()`

Las pruebas unitarias significativas son la siguientes:

```
it('should call Users.query when the view changes', function() {
   testInput.$setViewValue('different');
   expect(Users.query).toHaveBeenCalled();
});
it('should set model to invalid if the Users.query response contains users', function() {
   testInput.$setViewValue('different');
   Users.respondWith(['someUser']);
   expect(testInput.$valid).toBe(false);
});
it('should set model to valid if the Users.query response contains no users', function() {
   testInput.$setViewValue('different');
   Users.respondWith([]);
   expect(testInput.$valid).toBe(true);
});        
```

Estamos comprobando para ver si el `Users.query()` fue llamado. También, ya que `Users.query()` sigue la retrollamada de respuesta, podemos simular una respuesta desde el servidor con Usuarios. `respondWith()`.

Una de las cuestiones que tenemos que probar, es que no queremos hacer solicitudes al servidor si el usuario re-ingresa el mismo valor proporcionado por el modelo. Por ejemplo, si estamos editando un usuario en lugar de crear un usuario, a continuación, e-mail original del usuario se encuentra en la base de datos del servidor, pero es una dirección de correo electrónico válida.

```
it('should not call Users.query if the view changes to be the same as the original model', function() {
   $scope.model.testValue = 'admin@abc.com';
   $scope.$digest();
   testInput.$setViewValue('admin@abc.com');
   expect(Users.query).not.toHaveBeenCalled();
   testInput.$setViewValue('other@abc.com');
   expect(Users.query).toHaveBeenCalled();

   querySpy.reset();
   testInput.$setViewValue('admin@abc.com');
   expect(Users.query).not.toHaveBeenCalled();
   $scope.model.testValue = 'other@abc.com';
   $scope.$digest();
   testInput.$setViewValue('admin@abc.com');
   expect(Users.query).toHaveBeenCalled();
});
```

Establecemos el modelo y luego comprobamos que `User.query()` es llamada sólo si el valor del campo es establecida a un correo que no concuerde con el valor del modelo original. Usamos `Users.query.reset()` cuando queremos comprobar que el observador no ha sido llamado ya que la ultimas vez nosotros comprobamos.

### Implementado la directiva de validación asíncrona

La implementación de esta directiva es similar en estructura a la previa directiva de validación. Nosotros requerimos el controlador ngModel y añadimos a las $parsers y $formatters en la función enlazada:

```
myModule.directive('uniqueEmail', ["Users", function (Users) {
   return {
       require:'ngModel',
       link:function (scope, element, attrs, ngModelCtrl) {
           var original;
           ngModelCtrl.$formatters.unshift(function(modelValue) {
               original = modelValue;
               return modelValue;
           });
           ngModelCtrl.$parsers.push(function (viewValue) {
               if (viewValue && viewValue !== original ) {
                   Users.query({email:viewValue}, function (users) {
                       if (users.length === 0) {
                           ngModelCtrl.$setValidity('uniqueEmail', true);
                       } else {
                           ngModelCtrl.$setValidity('uniqueEmail', false);
                       }
                   });
                   return viewValue;
               }
           });
       }
   };
}]);
```

Nosotros solo estamos comprobando con el servidor en la función `$parser`, es decir, cuando el usuario cambia el campo. Si el valor es actualizado programáticamente, vía el modelo, nosotros asumimos que la lógica de negocio de la aplicación asegura que es una dirección de correo válida. Por ejemplo, si cargamos un usuario existente para editar, la dirección de correo es válida a pesar de que ya está en uso.

Normalmente, en una funcion de validacion usted retorna `undefined` si el valor no es válido. Esto previene que el modelo sea actualizado con un valor inválido. En esta caso, en el punto de retorno, la funcion de validacion no sabe si el valor es válido o no. Entonces retornamos el valor de cualquier forma y dejamos que la retrollamada de respuesta establezca la validación luego.

Estamos usando la series de funciones formateadoras (`$formatters`) para añadir una función que siga el valor original que fué establecido en el modelo. Esto previene que la funcion de validacion contacte con el servidor si el usuario re-introduce la dirección de correo original, como sería incorrecto establecer el correo como inválido.

## Envolviendo la directiva datepicker de jQueryUI

Algunas veces hay widget de terceros que son lo suficientemente complejos y no vale la pena escribir una versión AngularJS pura de ellos en corto tiempo.  Usted puede acelerar su desarrollo envolviendolo tal como un widget en una directiva AngularJS pero hay que tener cuidado con la forma en las dos bibliotecas interactuarían.

Aquí vamos a ver la fabricación de una directiva de un campo `datepicker` que envuelva el widget datepicker de jQueryUI. El widget expone la siguiente API que usaremos para integrarlo dentro dentro de AngularJS, donde el elemento es la envoltura jQuery alrededor del elemento en el cual el widget se va a unir.

![chapter-8-6](/img/chapter-8-6.png)

Queremos ser informados cuando el usuario seleccione una nueva fecha con el selector. La opción que pasemos para crear una nuevo widget puede proporcionar una retrollamada onSelect que será llamada cuando el usuario seleccione una fecha:

```
element.datepicker({onSelect: function(value, picker) { ... });
```

> `NOTA` Para mantener las cosas simples, espeficicaremos que la directiva que la directiva datepicker puede solo ser enlazada a un objeto Date JavaScript en el modelo.

El patrón general para envolver el campo JQuery widgets es similar, de nuevo, a la construcción de una directiva de validación. Necesitas `ngModel` y colocar funciones en el conjunto `$parsers` y `$formatters` para transformar el valor entre el modelo y la vista.

También, necesitamos introducir el widget donde el modelo cambia y obtener datos dentro del modelo cuando el widget cambie. Re-escribiremos `ngModel.$render()` para actualizar el widget. Esta función es llamada luego que todos los `$formatters` han sido ejecutados satisfactoriamente. Para obtener la salida de datos, usaremos la retrollamada  onSelect para llamar `ngModel.$setViewValue()`, el cual actualiza el valor de la vista y despliega la serie de `$parsers`.

![chapter-8-7](/img/chapter-8-7.png)

### Escribir pruebas para directivas que envuelven librerías

En una prueba unitaria pura podemos crear un simulador widget datepicker jQueryUI que exponga la misma interfaz. En ese caso vamos a tomar un enfoque más pragmático y usaremos un widget datepicker real en las pruebas.

La ventaja de esto es que no tenemos que depender en la interfaz del widget's que esta documentada con precisión. Llamando los métodos actuales y comprobar que la interfaz de usuario es actualizada correctamente, podemos estar muy seguros de que nuestra directiva está trabajando.  Las desventajas son que la manipulación del DOM en el widget puede desacelerar la ejecuciones de pruebas y debe haber una forma de interactuar con el widget para asegurar que esta se esta comportando correctamente.

En este caso, el widget datepicker jQueryUI expone otra función que nos permite simular un usuario seleccionando una fecha.

```
$.datepicker._selectDate(element);
```

Creamos una función ayudante `selectDate()`, la cual usaremos para simular la selección de la fecha en el widget:

```
var selectDate = function(element, date) {
   element.datepicker('setDate', date);
   $.datepicker._selectDate(element);
};
```

> `NOTA`: Este tipo de simulación es algunas veces difíciles de lograr,  y si es así,  usted debe considerar simular la salida del widget por completo.   

Las pruebas en sí hacen uso de la API widget's y de esta función ayudante. Por ejemplo:

```
describe('simple use on input element', function() {
   var aDate, element;
   beforeEach(function() {
       aDate = new Date(2010, 12, 1);
       element = $compile(
           "<input date-picker ng-model='x'/>")($rootScope);
   });
   it('should get the date from the model', function() {
       $rootScope.x = aDate;
       $rootScope.$digest();
       expect(element.datepicker('getDate')).toEqual(aDate);
   });
   it('should put the date in the model', function() {
       $rootScope.$digest();
       selectDate(element, aDate);
       expect($rootScope.x).toEqual(aDate);
   });
});
```

Aquí, comprobamos que los cambios en el modelo son desviados a el widget y que los cambios en widget se pasan de nuevo al modelo. Nota que no llamamos `$digest()` luego de `selectDate()`, ya que es trabajo de la directiva asegurarse que la digestión ocurra luego de una interacción del usuario.  

> `NOTA`: Hay más pruebas para todos los diferentes escenarios para esta directiva. Ellos pueden ser encontrados en el código de ejemplo.    

### Implementando la directiva datepicker jQuery

La implementación de la directiva es de nuevo hacer uso del la funcionalidad proporcionada por el `ngModelController`. En particular, nosotros añadimos una función a la serie de `$formatters` que aseguran que el modelo es un objeto `Date`, nosotros añadimos nuestra retrollamada `onSelect` a las opciones, y reescribimos la función `$render` para actualizar el widget cuando el modelo cambie.

```
myModule.directive('datePicker', function () {
   return {
       require:'ngModel',
       link:function (scope, element, attrs, ngModelCtrl) {
           ngModelCtrl.$formatters.push(function(date) {
               if ( angular.isDefined(date) &&
                   date !== null &&
                   !angular.isDate(date) ) {
                   throw new Error('ng-Model value must be a Date object');
               }
               return date;
           });
           var updateModel = function () {
               scope.$apply(function () {
                   var date = element.datepicker("getDate");
                   element.datepicker("setDate", element.val());
                   ngModelCtrl.$setViewValue(date);
               });
           };
           var onSelectHandler = function(userHandler) {
               if ( userHandler ) {
                   return function(value, picker) {
                       updateModel();
                       return userHandler(value, picker);
                   };
               } else {
                   return updateModel;
               }
           };
```

El manejador `onSelect()` llama nuestra función `updateModel()`, la cual para el nuevo valor date dentro de la serie de `$parsers` vía `$setViewValue()`:

```
           var setUpDatePicker = function () {
               var options = scope.$eval(attrs.datePicker) || {};
               options.onSelect = onSelectHandler(options.onSelect);
               element.bind('change', updateModel);
               element.datepicker('destroy');
               element.datepicker(options);
               ngModelCtrl.$render();
           };
           ngModelCtrl.$render = function () {
               element.datepicker("setDate", ngModelCtrl.$viewValue);
           };
           scope.$watch(attrs.datePicker, setUpDatePicker, true);
       }
   };
});
```

## Resumen:

En este capítulo observamos una variedad de patrones comunes para definir, probar, y implementar directivas. Vimos cómo integrar con el `ngModel` para implementar validación, como escribir un widget encapsulado reusable. A lo largo del capítulo las pruebas han sido promovidas y observamos las estrategias comunes para probar directivas en AngularJS.

En el próximo capítulo vamos a echar un vistazo más profundo en la construcción de directivas, observando algunos de las más avanzadas características tales como transclusión, y compilar nuestras propias plantillas.