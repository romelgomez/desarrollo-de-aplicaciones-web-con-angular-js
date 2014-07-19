## Capítulo 1. Angular Zen

Este capítulo sirve como una introducción a AngularJS, tanto el marco como el proyecto detrás de él. En primer lugar vamos a echar un breve vistazo a el proyecto en si; quien lo conduce, dónde encontrar el código fuente y la documentación, como pedir ayuda, y así sucesivamente.

La mayor parte de este capítulo es una introducción al marco AngularJS, son conceptos básicos y patrones de código. Hay una gran cantidad de material que cubrir, así que para hacer el proceso de aprendizaje rápido y sin complicaciones, hay un montón de ejemplos de código.

AngularJS es un marco único que sin duda le dará forma al área del desarrollo web en los próximos años. Es por eso que la última parte de este capítulo explica qué hace AngularJS tan especial, cómo se compara con otros marcos existentes, y lo que podemos esperar de él en el futuro.

En este capítulo se cubren los siguientes temas:

- Cómo escribir una simple aplicación Hola Mundo en AngularJs. En el proceso de hacerlo, se llegara a saber dónde encontrar el código fuente del marco, la documentación y la comunidad.
- Familiarizarse con los elementos básicos de cualquier aplicación AngularJS: plantillas con  directivas, ámbitos y controladores.
- Tomar conciencia de del sistema de inyección de dependencia sofisticado AngularJS con todos sus matices.
- Para entender cómo AngularJS se compara con otros marcos y librerías (especialmente jQuery) y que hace que sea tan especial.

## Conocé AngularJS

AngularJS es un marco del lado del cliente MVC escrito en JavaScript. Se ejecuta en un navegador web y nos ayuda a escribir modernas aplicaciones web, de una sola página, al estilo AJAX. Es un marco de propósito general, pero brilla cuando se utiliza para escribir aplicaciones web del tipo CRUD (Create Read Update Delete).

### Familiarizarse con el marco

AngularJS es una adición reciente a la lista de marcos MVC del lado del cliente, ha logrado atraer mucha atención, sobre todo debido a su innovador sistema de plantillas, facilidad de desarrollo y prácticas de ingeniería muy sólidas. En efecto, su sistema de plantillas es único en muchos aspectos:

- Utiliza HTML como el lenguaje de plantillas.
- No requiere una explícita actualización del DOM, dado que AngularJS es capaz de rastrear las acciones del usuario, navegador, eventos y los cambios del modelo, como para saber cuándo y qué plantillas refrescar.
- Tiene un muy interesante y extensible subsistema de componentes y es posible enseñar a un explorador cómo interpretar nuevas etiquetas y atributos HTML.

El subsistema de plantillas puede ser la parte más visible de AngularJS pero no te dejes confundir ya que es angularJs es un marco completo lleno de varias utilidades y servicios normalmente necesarios en las aplicaciones web de una sola página.

AngularJs también tiene algunos tesoros escondidos, **inyección de dependencias** (DI) y un fuerte enfoque en la verificabilidad. El soporte incorporado para la inyección de dependencias hace que sea fácil de ensamblar aplicaciones web menores, a través de los servicios exhaustivamente probados. El diseño de la estructura y las herramientas a su alrededor promueven prácticas de prueba en cada etapa del proceso de desarrollo.

### Encuentre su camino en el proyecto

AngularJS es relativamente un nuevo actor en la escena de los marcos MVC del lado del cliente, su versión 1.0 fue lanzada en junio de 2012. En realidad, el trabajo en este marco se inició en 2009 como un proyecto personal de Miško Hevery, un empleado de Google. La idea inicial resultó ser tan buena que, al momento de empezar a escribir, el proyecto fue oficialmente respaldado por Google Inc, y hay todo un equipo a Google trabajando a tiempo completo en el marco.

AngularJS es un proyecto de código abierto alojado en GitHub (https://github.com/angular/angular.js) y licenciado por Google, Inc., en los términos de la licencia MIT.

### La comunidad

Al final del día, ningún proyecto podría sobrevivir sin gente de pie detrás de él. Afortunadamente, AngularJS tiene una gran comunidad de apoyo. Los siguientes son algunos de los canales de comunicación donde se puede discutir temas de diseño y solicitar ayuda:

- lista de correo angular@googlegroups.com (grupo Google)
- Google + comunidad en https://plus.google.com/u/0/communities/115368820700870330756
- \# angularjs canal IRC
- [angularjs] tag en http://stackoverflow.com


Los equipos AngularJS se mantienen en contacto con la comunidad mediante el blog (http://blog.angularjs.org/) y al estar presente en los medios de comunicación social, Google+ (+AngularJS) y Twitter (@angularjs). También hay reuniones de comunidades que se organizan en todo el mundo, si alguna pasa cerca de algún lugar de donde vive, definitivamente vale la pena asistir!

### Recursos de aprendizaje en línea

AngularJS tiene su propia página web (http://www.angularjs.org) donde puede encontrar todo lo que uno esperaría de un marco respetable: resumen conceptuales, tutoriales, guía para el desarrollador, referencia de la API, y así sucesivamente. El código fuente para todas versiones liberadas de AngularJS pueden ser descargados de http://code.angularjs.org.

Las personas en busca de ejemplos de código no se sentirán decepcionados, ya que la documentación de AngularJS en sí tiene un montón de fragmentos de código. Además de esto, podemos ver una Galería de aplicaciones construidas con AngularJS (http://builtwith.angularjs.org). Un canal de YouTube dedicado (http://www.youtube.com/user/angularjs) que tiene grabaciones de muchos eventos pasados, así como algunos videos tutoriales muy útiles.

### Bibliotecas y extensiones

Si bien núcleo AngularJS está lleno de funcionalidades, la comunidad se mantiene activamente agregando nuevas extensiones casi todos los días. Muchos de ellas se listan en una página web dedicada: http://ngmodules.org.

### Herramientas

AngularJS es construido encima del HTML y JavaScript, dos tecnologías que hemos estado utilizando en el desarrollo web desde hace años. Gracias a esto, se pueden seguir usando nuestros editores favoritos e IDEs, extensiones del navegador, etc sin ningún problema. Además, la comunidad AngularJS ha contribuido con varios e interesantes complementos para caja de herramientas HTML/JavaScript ya existente.

### Batarang

Batarang es una extensión de la herramienta de desarrollo Chrome para la inspección de aplicaciones web AngularJS. Batarang es muy útil para visualizar y analizar en tiempo de ejecución características de las aplicaciones AngularJS. Vamos a utilizar batarang ampliamente en este libro, para mirar bajo el capó de una aplicación en ejecución. Batarang se puede instalar desde la tienda web de Chrome (*AngularJS batarang*) como cualquier otra extensión de Chrome.

### Plunker y jsFiddle

Tanto Plunker (http://plnkr.co) y jsFiddle (http://jsfiddle.net) hacen muy fácil de compartir fragmentos de código en vivo (JavaScript, CSS y HTML). Si bien estas herramientas no están reservadas exclusivamente para el uso con AngularJS, fueron rápidamente adoptadas por la comunidad AngularJS para compartir ejemplos de código pequeños, escenarios para reproducir errores, y así sucesivamente. Plunker merece mención especial, ya que fue escrito en AngularJS, y es una herramienta muy popular en la comunidad.

### Extensiones y plugins IDE

Cada uno de nosotros tiene un IDE favorito o un editor. La buena noticia es que hay plugins/extensiones existentes para varios entornos de desarrollo populares, tales como Sublime Text 2 (https://github.com/angular-ui/AngularJS-sublime-package), productos Jet Brains (http://plugins.jetbrains.com/plugin?pr=idea&pluginId=6971), y así sucesivamente.

## Curso intensivo AngularJS

Ahora que sabemos dónde encontrar la librería y su documentación anexa, podemos empezar a escribir el código para ver realmente AngularJS en acción. Esta sección del libro sienta las bases para los capítulos posteriores por cubrir: plantillas AngularJS, modularidad, y la inyección de dependencia. Esos son los elementos básicos de construcción de cualquier aplicación web AngularJS.

### Hola Mundo - el ejemplo AngularJS

Vamos a echar un vistazo al típico ejemplo "¡Hola, mundo!" escrito en AngularJS para obtener la primera impresión de la estructura y la sintaxis que emplea.

```html
<html>
<head>
  <script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.0.7/angular.js"></script>
</head>
<body ng-app ng-init="name = 'World'">
  <h1>Hello, {{name}}!</h1>
</body>
</html>
```

En primer lugar, tenemos que incluir la biblioteca AngularJS para que nuestro ejemplo funcione correctamente en un navegador web. Es muy fácil como AngularJS, en su forma más simple, es empaquetado como un único archivo JavaScript

> `NOTA` La biblioteca AngularJS es una relativamente pequeña versión miniaturizada y comprimida que tiene un tamaño de alrededor de 30 KB. La versión miniaturizada sin compresión gzip tiene un tamaño de alrededor de 80 KB. No requiere ninguna dependencia de terceros.
>
> Para los ejemplos cortos de este libro vamos a utilizar una versión no miniaturizada, amistosa con el desarrollador, alojada en la red de entrega de contenido de Google (CDN). También el código fuente de todas las versiones de AngularJS también puede ser descargado desde http://code.angularjs.org.

Incluir la biblioteca AngularJS no es suficiente para tener un ejemplo en ejecución. Necesitamos arrancar nuestra mini aplicación. La manera más fácil de hacer esto es mediante el uso de un atributo HTML personalizado `ng-app`.

Una inspección más cercana de la etiqueta `<body>` revela otro atributo HTML no estándar: `ng-init`. Podemos utilizar `ng-init` para inicializar modelo antes que la plantilla sea presentada. El último punto por cubrir es la expresión `{{name}}` la cual simplemente hace visible el valor del modelo. 

Incluso este sencillo primer ejemplo pone de manifiesto algunas características importantes del sistema de plantillas AngularJS, que son las siguientes:

- Etiquetas HTML y atributos personalizados se utilizan para añadir comportamiento dinámico a un documento HTML que de otro modo sería estático.

- Se utiliza llaves dobles `{{expresión}}` como un delimitador de expresiones donde se emiten valores de modelo.

En las aplicaciones AngularJS, todas las etiquetas y atributos HTML especiales que el marco puede comprender e interpretar se conocen como directivas.

### Enlace de datos de doble vía

El renderizado de una plantilla es sencillo con AngularJS, el marco brilla cuando se utiliza para crear aplicaciones web dinámicas. Con el fin de apreciar el verdadero poder de AngularJS, vamos a ampliar nuestro "Hola Mundo" con un campo de entrada, como se muestra en el siguiente código:

```html
<body ng-app ng-init="name = 'World'">
 Say hello to: <input type="text" ng-model="name">
 <h1>Hello, {{name}}!</h1>
</body>
```

No hay casi nada de especial en la etiqueta HTML `<input>` además del atributo adicional ng-model. La verdadera magia ocurre tan pronto como empezamos a escribir texto en el campo `<input>`. De repente, la pantalla es re-pintada después de cada golpe de tecla, para reflejar el nombre proporcionado! No hay necesidad de escribir ningún código para actualizar una plantilla, y no estamos obligados a hacer llamadas a la API de cualquier marco para a actualizar el modelo. AngularJS es lo suficientemente inteligente como para detectar cambios en el modelo y actualizar el DOM en consecuencia.

La mayor parte de los sistemas tradicionales de plantilla renderiza plantillas en un proceso lineal de un solo sentido: un modelo (las variables) y una plantilla se combinan entre sí para producir un documento marcado. Cualquier cambio en el modelo requiere la reevaluación de una plantilla. AngularJS es diferente porque los cambios en la vista provocados por un usuario son inmediatamente reflejados en el modelo, y cualquier cambio en el modelo se propagan instantáneamente a una plantilla.

### El patrón MVC en AngularJS

La mayoría de las aplicaciones web existentes se basan en alguna forma del conocido patrón **modelo-vista-controlador (MVC)**. Pero el problema con el MVC es que se no es un patrón muy preciso, sino más bien una, una arquitectura de alto nivel. Peor aún, hay muchas variaciones existentes y derivados del patrón original (MVP y MVVM parecen ser los más populares). Para agregar más a la confusión, diferente marcos y desarrolladores tienden a interpretar los patrones mencionados de forma diferente. Esto da lugar a situaciones en las que se utiliza el mismo nombre de MVC para describir diferentes arquitecturas y enfoques de codificación. *Martin Fowler* lo resume muy bien en su excelente artículo sobre arquitecturas GUI (http://martinfowler.com/eaaDev/uiArchs.html):

> *Tome el Modelo-Vista-Controlador como un ejemplo. Es a menudo referido como un patrón, pero yo no lo encuentro tremendamente útil para pensar en él como un patrón, ya que contiene un buen número de ideas diferentes. Diferentes personas leyendo sobre MVC en diferentes lugares tienen diferentes ideas del MVC. Si esto no le causa suficiente confusión a continuación, puedes lograr la sensación sobre los malentendidos del MVC al desarrollar a través de un sistema de susurros chinos.*

El equipo AngularJS adopta un enfoque muy pragmático para toda la familia de patrones del MVC, y declara que el marco se basa en el patrón MVW (model-view-whatever) (modelo-vista-cualquier cosa). Básicamente hay que verlo en acción para conseguir la sensación de ello.

### Vista panorámica

Todos los ejemplos de "Hola Mundo" que hemos visto hasta ahora no emplean ninguna estrategia de capas explícita: la inicialización de datos, la lógica, y las vistas fueron todos mezclados en un solo archivo. En cualquier aplicación del el mundo real, sin embargo, tenemos que prestar más atención al conjunto de responsabilidades asignadas a cada capa. Afortunadamente, AngularJS ofrece diferentes esquemas arquitectónicos que nos permite construir adecuadamente las aplicaciones más complejas.

> `NOTA` Todos los ejemplos subsiguientes lo largo del libro omiten el Código de inicialización AngularJS (la inclusión de scripts, el atributo `ng-app`, y así sucesivamente) para mejorar la legibilidad.

Vamos a echar un vistazo al "Hola Mundo" ligeramente modificado:

```
<div ng-controller="HelloCtrl">
  Say hello to: <input type="text" ng-model="name"><br>
  <h1>Hello, {{name}}!</h1>
</div>
```

Se eliminó el atributo `ng-init`, y en su lugar se puede ver un nuevo atributo o directiva `ng-controller` con el nombre de la una función JavaScript correspondiente. La función `HelloCtrl` acepta un misterioso argumento $scope tal y como se muestra en el siguiente código:

```
var HelloCtrl = function ($scope) {
 $scope.name = 'World';
}
```

### Ámbito 

Un objeto `$scope` en AngularJS está aquí para exponer el modelo de dominio a la vista (la plantilla). Mediante la asignación de propiedades a las instancias del ámbito, podemos hacer que nuevos valores estén disponibles para una plantilla para ser representados.

Los ámbitos se pueden aumentar tanto con los datos y la funcionalidad específica para una vista determinada. Podemos exponer la lógica de la interfaz de usuario específica a las plantillas mediante la definición de las funciones en una instancia del ámbito.

Por ejemplo, se podría crear una función getter para la variable `name`, tal como figura en el siguiente código:

```
var HelloCtrl = function ($scope) {
  $scope.getName = function() {
    return $scope.name;
  };
}
```

Y luego usarlo en una plantilla, tal como se da en el siguiente código:

```
<h1>Hello, {{getName()}}!</h1>
```

El objeto `$scope` nos permite controlar con precisión qué parte del modelo de dominio y qué operaciones están disponibles para las capas de vista. Conceptualmente, los ámbitos AngularJS están muy cerca de los ViewModel del patrón MVVM.

### Controlador

La principal responsabilidad de un controlador es inicializar los objetos del ámbito. En la práctica, la lógica de inicialización consta de las siguientes responsabilidades:

- Proporcionar los valores iniciales del modelo
- Extender `$scope` con funciones o comportamientos específicos de la interfaz de usuario.

Los controladores son funciones regulares de JavaScript. Ellos no tienen que extender ninguna clases específicas del marco ni llamar a cualquier API AngularJS en particular para realizar su trabajo correctamente.

>`NOTA` Tenga en cuenta que un controlador hace el mismo trabajo que la directiva `ng-init`, a la hora de establecer los valores iniciales del modelo.  
>
> Controladores hacen que sea posible expresar esta lógica de inicialización en JavaScript, sin abarrotar las plantillas HTML con código.


### Modelo

Modelos AngularJS son simples, viejos objetos JavaScript. Nosotros no estamos obligados a extender cualquiera de las clases base del marco ni construir objetos del modelo de ninguna manera especial.

Es posible tomar alguna de la existentes, clases puras JavaScript o objetos y utilizarlos de la misma manera como una capa de modelo. No estamos limitados a las propiedades del modelo que estén representados en valores primitivos (cualquier objeto JavaScript válido o una matriz se pueden utilizar). Para exponer un modelo a AngularJS simplemente asigna el modelo al `$scope`.

> `NOTA` AngularJS no es intrusivo y nos permite mantener los objetos del modelo libre de cualquier código específico del marco.

### Ámbitos en profundidad

Cada `$scope` es una instancia de la clase Scope. La clase `Scope` tiene métodos que controla el ciclo de vida del los ámbitos, provee la facilidad de propagación de eventos, y da soporte al proceso de preparar la plantilla.

### Jerarquía de ámbitos

Vamos a echar otro vistazo al ejemplo sencillo `HelloCtrl`, que hemos examinado ya:

```
var HelloCtrl = function ($scope) {
  $scope.name = 'World';
}
```

El `HelloCtrl` se parece a una función JavaScript regular, no hay absolutamente nada de especial aparte del argumento `$scope`. ¿De dónde podría este argumento venir?

Un nuevo ámbito fue creado por el método `Scope.$new()` al usar la directiva `ng-controller`. Espere un momento, parece que tenemos que tener por lo menos una instancia de un ámbito para crear un nuevo ámbito! En efecto, AngularJS tiene en agenda el ámbito `$rootScope` (el ámbito que es padre de todos los otros ámbitos). La instancia `$rootScope` consigue ser creada cuando se inicializa una nueva aplicación.

La directiva `ng-controller` es un ejemplo de una directiva que crea un nuevo ámbito. AngularJS creará una nueva instancia de la clase Scope siempre que encuentre una directiva que cree un nuevo ámbito en el árbol DOM. Un ámbito recién creado apunta a su ámbito ascendiente usando la propiedad `$parent`. Puede haber muchas directivas que crean un nuevo ámbito en el árbol DOM y como resultado se crearán muchos ámbitos.

> `NOTA`  Los ámbitos forman una relación padre-hijo, semejante a la relación que se ve en el árbol cuya raíz es la instancia `$rootScope`. La creación de ámbitos es impulsada por el árbol DOM, no es de extrañar que la estructura tipo árbol de los ámbitos imitan la estructura DOM

Ahora que sabemos que algunas directivas crean nuevos ámbitos descendientes, puede que se pregunte por qué es necesaria toda esta complejidad. Para entender esto, vamos a echar un vistazo al ejemplo que hace uso de una directiva repetidora: `ng-repeat`.

El controlador es el siguiente:

```
var WorldCtrl = function ($scope) {
  $scope.population = 7000;
  $scope.countries = [
    {name: 'France', population: 63.1},
    {name: 'United Kingdom', population: 61.8},
  ];
};
```

Y el fragmento marcado se ve de la siguiente manera:

```
<ul ng-controller="WorldCtrl">
  <li ng-repeat="country in countries">
    {{country.name}} has population of {{country.population}} 
  </li>
  <hr>
  World's population: {{population}} millions
</ul>
```

La directiva `ng-repeat` nos permite iterar sobre una colección de países y crear nuevos elementos DOM para cada elemento del array. La sintaxis de la directiva `ng-repeat` debe ser fácil de seguir, una nueva variable `country` se crea para cada elemento y es expuesta en el `$scope` (ámbito) para ser presentada en la vista. 

Pero hay un problema aquí, es que, una nueva variable necesita ser expuesta en el `$scope` (ámbito) para cada país y no podemos simplemente re-escribir los anteriores valores expuestos. AngularJS resuelve este problema mediante la creación de un nuevo ámbito para cada elemento de la colección. Los ámbitos recién creados formarán una jerarquía que sigue de muy cerca la estructura del árbol DOM, la cual podemos visualizar mediante el uso de la excelente extensión batarang para Google Chrome tal como se muestra en la siguiente captura de pantalla:

![chapter-1-1](/img/chapter-1-1.png)

Como podemos ver en la captura de pantalla, cada ámbito (está marcado por los límites señalados con un rectángulo rojo) posee su propio conjunto o modelo de valores. Es posible definir la misma variable en diferentes ámbitos sin crear conflictos de nombres (los diferentes elementos del DOM simplemente apuntan a diferentes ámbitos y usan las variables del ámbito correspondiente para renderizar una plantilla).  De esta manera cada elemento tiene su propio espacio de nombres, en el ejemplo anterior cada elemento `<li>` tiene su propio ámbito donde la variable país puede ser definida.

### Jerarquía de ámbitos y la herencia

Las propiedades que son definidas en un ámbito son visibles para todos los ámbitos dependientes, siempre y cuando el ámbito dependiente no redefina la propiedad usando el mismo nombre, esto es muy útil en la práctica, ya que no es necesario definir una y otra vez las propiedades que debén estar disponible a lo largo de una jerarquía de ámbitos. 

Basándose en el ejemplo anterior, supongamos que queremos mostrar el porcentaje de la población mundial que vive en un país determinado. Para ello, podemos definir la función worldsPercentage en un ámbito gestionado por el controlador `WorldCtrl` tal como figura en el siguiente código:  

```
$scope.worldsPercentage = function (countryPopulation) {
  return (countryPopulation / $scope.population)*100;
}
```

Y a continuación, se llama a esta función para cada instancia del ámbito creada por la directiva `ng-repeat` de la siguiente manera:

```
<li ng-repeat="country in countries">
  {{country.name}} has population of {{country.population}},
  {{worldsPercentage(country.population)}} % of the World's population
</li>
```

La jerarquía de los ámbitos en AngularJS sigue las mismas reglas de la herencia prototípica en JavaScript (cuando tratamos de leer una propiedad, se puede recorrer el árbol hacia arriba hasta que la propiedad sea encontrada)

### Los peligros de la herencia a través de la jerarquía de ámbitos

La herencia a través de la jerarquía de ámbitos es bastante intuitiva y fácil de entender a la hora de tener acceso a leer. Sin embargo, a la hora de tener acceso de escritura, las cosas se vuelven un poco más complicadas.

Vamos a ver lo que sucede al definir una variable en un ámbito y luego ver como es omitida si proviene de un ámbito dependiente. El código JavaScript es la siguiente:

```
var HelloCtrl = function ($scope) {
};
```

Y el código de la vista es el siguiente:

```
<body ng-app ng-init="name='World'">
  <h1>Hello, {{name}}</h1>
  <div ng-controller="HelloCtrl">
    Say hello to: <input type="text" ng-model="name">
    <h2>Hello, {{name}}!</h2>
  </div>
</body>
```

Si intenta ejecutar este código, se dará cuenta que la variable `name` es visible en toda la aplicación, incluso si es definida solamente en el ámbito superior!. Esto ilustra que las variables son heredadas hacia abajo en la jerarquía de ámbitos. En otra palabras , las variables definidas en un ámbito ascendiente son accesibles en los ámbitos descendientes. 

Ahora, vamos a observar lo que pasará si empezamos a escribir texto en el campo `<input>`, como se muestra en la siguiente captura de pantalla:

![chapter-1-2](/img/chapter-1-2.png)

Usted puede estar algo sorprendido de ver como la nueva variable creada en el ámbito del controlador `HelloCtrl`, en lugar de cambiar el valor establecido en la instancia `$rootScope` solo cambia el valor establecido en el ámbito inicializado por el controlador `HelloCtrl`. Este comportamiento se ve menos sorprendente cuando nos damos cuenta de que los ámbitos prototípicamente heredan entre sí. Todas las reglas que se aplican a la herencia prototípica de los objetos en JavaScript se aplican igualmente a los ámbitos que heredan prototípicamente. Los ámbitos son sólo objetos JavaScript después de todo. 

Hay varias formas de afectar las propiedades definidas en un ámbito ascendente desde un ámbito descendiente. Primero, se podría hacer referencia explícitamente al ámbito ascendente usando la propiedad `$parent`. Una plantilla modificada se vería de la siguiente manera:  

```
<input type="text" ng-model="$parent.name">
```

Si bien es posible resolver el problema usando este ejemplo, haciendo referencia directamente al ámbito ascendente, tenemos que darnos cuenta de que esta es una solución muy frágil. El problema es que la expresión utilizada por la directiva `ng-model` hace una fuerte suposición sobre toda la estructura DOM. Y solo es suficiente insertar otra directiva creadora de ámbitos en algún lugar por encima de la etiqueta `<input>` para que `$parent` apunte completamente a un ámbito distinto.

> `NOTA` Como regla general, trate de evitar el uso de la propiedad `$parent` ya que enlaza fuertemente expresiones AngularJS a la estructura DOM creadas por sus plantillas. Una aplicación puede romperse con facilidad como consecuencia de cambios simples en su estructura HTML.

Otra solución consiste crear la unión en función a una propiedad de un objeto del ámbito y no directamente a una propiedad del ámbito. El código para esta solución es el siguiente:


```
<body ng-app ng-init="thing = {name : 'World'}">
  <h1>Hello, {{thing.name}}</h1>
  <div ng-controller="HelloCtrl">
    Say hello to: <input type="text" ng-model="thing.name">
    <h2>Hello, {{thing.name}}!</h2>
  </div>
</body>
```

Este enfoque es mucho mejor, ya que no asume nada acerca de la estructura de árbol DOM.

> `TIP` Evita crear enlaces directos a propiedades del ámbito. Es preferible crear enlaces hacia propiedades del objeto del ámbito. Como regla general, la expresión proporcionada debe tener un punto intermedio, como se observa en la directiva `ng-model` (`ng-model="thing.name"`).

### La Jerarquía de los ámbitos y el sistema de eventos

Ámbitos organizados en una jerarquía se pueden utilizar como un bus de eventos. AngularJS nos permite propagar eventos con carga a través de la jerarquía de ámbitos. Un evento puede ser enviado a partir de cualquier ámbito y viajar tanto para arriba (`$emit`) o como hacia abajo (`$broadcast`).

![chapter-1-3](/img/chapter-1-3.png)

Los servicios básicos y directivas AngularJS hacen uso de este bus de eventos para señalar los cambios importantes en el estado de la aplicación. Por ejemplo, podemos escuchar el evento `$locationChangeSuccess` (transmitido desde la instancia `$rootScope` ) para ser notificados cada vez que una ubicación (URL en un navegador) cambia, tal como se indica en el siguiente código:

```
$scope.$on('$locationChangeSuccess', function(event, newUrl, oldUrl){
  // reaccionar ante el cambio de ubicación aquí
  // por ejemplo, actualiza el breadcrumbs en base a la newUrl (nueva URL)
});
```

El método `$on` está disponible para cada instancia de cada ámbito, este puede ser llamado para registrar un gestor de un evento de tal ámbito. Una función actúa como gestor la cual será invocada con el objeto del evento designado como primer argumento. Los argumentos subsiguientes corresponden a la carga útil del evento y dependen del tipo de evento.
 
Al igual que los eventos DOM, podemos llamar al método `preventDefault()` y `stopPropagation()` en objeto del evento. El método `stopPropagation()` puede ser llamado para prevenir que un evento burbujee hacia arriba a través de la jerarquía de ámbitos desde el ámbito donde se produce el evento y este método está disponible sólo para eventos que se despliegen hacia arriba en la jerarquía de ámbitos (`$emit`). 

> `NOTA` Si bien el sistema de eventos de AngularJS es modelado luego del DOM, ambos sistemas de propagación de eventos son totalmente independientes y no tienen partes comunes. 

Si bien estos eventos propagados por medio de la jerarquía de ámbitos son una solución elegante a muchos problemas (especialmente cuando se trata de notificaciones relacionadas con los cambios de estados globales asíncronos), estos deben ser usados con moderación. Por lo general, podemos confiar en el enlace de datos de doble vía para lograr una solución más limpia. En todo el marco AngularJS, solo hay tres eventos que emiten hacia arriba, estos son:  (`$includeContentRequested`, `$includeContentLoaded`, `$viewContentLoaded`), y siste eventos que se emiten hacia abajo, estos son:  (`$locationChangeStart`, `$locationChangeSuccess`, `$routeUpdate`, `$routeChangeStart`, `$routeChangeSuccess`, `$routeChangeError`, `$destroy`). Como puedes ver, los eventos de ámbitos se utilizan escasamente y nosotros debemos evaluar otras opciones (mayormente en el enlace de doble vía) antes de enviar eventos personalizado.  


> `TIP` No trate de imitar el modelo de programación basado en eventos DOM en AngularJS. La mayoría de las veces hay mejores maneras de estructurar su aplicación, y se puede ir muy lejos con el enlace de datos de doble vía.

### Ciclo de vida del ámbito

Los ámbitos son necesarios para proporcionar espacios de nombres aislados y así evitar colisiones de nombre de variables. Los ámbitos que son más pequeños y organizados en una jerarquía ayudan en la gestión de uso de la memoria. Cuando ya no se necesita uno de los ámbitos, este puede ser destruido. Como resultado, el modelo y la funcionalidad expuesta en este ámbito serán elegibles para la recolección de basura.

Nuevos ámbitos son usualmente traídos a la vida y destruidos por la directiva creadora de ámbitos (es decir que los ámbitos son destruidos por la propia directiva que los crea),  También es posible crear y destruir ámbitos llamando manualmente los métodos `$new()` y `$destroy()`, respectivamente (ambos métodos son definidos del tipo `Scope`).

### Vista

Ya hemos visto suficientes ejemplos de plantillas AngularJS, como para darnos cuenta de que no es aún otro lenguaje de plantillas, si no una cosa completamente diferente, el marco no solo nos permite extender el vocabulario HTML y además de usar HTML para la sintaxis de la plantillas, también tiene la capacidad única para actualizar partes de la pantalla sin necesidad alguna de intervención manual. 

En realidad, AngularJS tiene aún más estrechas conexiones con el HTML y el DOM ya que depende de un navegador para analizar el texto de la plantilla (como el navegador haría con cualquier otro documento HTML). Luego que de que un navegador termina de transformar el texto marcado en un árbol DOM, Angular se activa, atraviesa y analiza la estructura DON. Cada vez encuentra una directiva, AngularJs ejecuta su lógica para convertir las directivas en partes dinámicas de la pantalla.  

> `TIP` Como AngularJS depende de un navegador para analizar las plantillas, necesitamos asegurarnos que el lenguaje marcado represente HTML válido. Coloque especial atención en cerrar las etiquetas HTML correctamente (Si no lo hace, no producira ningun mensaje de error, solo que la vista no se representará correctamente). AngularJs trabaja usando el, árbol DOM válido en vivo!

Angular hace posible enriquecer el vocabulario HTML (nosotros podemos añadir nuevos atributos o elementos HTML y enseñar al navegador cómo debe interpretarlas). Es casi similar a la creación de un nuevo **lenguaje específico de dominio** (**DSL**) sobre el HTML e instruir a un navegador en la forma de dar sentido a las nuevas instrucciones. A menudo se oye que AngularJS "enseña a los navegadores nuevos trucos"

### Vista de plantilla declarativa  - la lógica del controlador imperativo

Hay muchas directivas útiles que ya vienen con AngularJS y nosotros vamos a cubrir la mayoría de las existentes en los siguientes capítulos. Lo que es probablemente lo más importante, sin embargo, no es la sintaxis y la funcionalidad de cada directiva, sino más bien la subyacente filosofía AngularJS de construcción de interfaces de usuario.

AngularJS promueve un enfoque declarativo en la construcción de la interfaz de usuario. Lo que significa en la práctica es que las plantillas se centran en describir el efecto deseado y no en las formas de lograrlo. Todo esto puede sonar un poco confuso, por lo que un ejemplo podría ser útil aquí.

Imaginemos que se nos pidió crear un formulario donde el usuario puede escribir un mensaje corto y, a continuación, enviarlo haciendo clic en un botón. Hay algunos requisitos adicionales de la **experiencia del usuario** (**UX**), tales como el tamaño del mensaje debe limitarse a 100 caracteres, y el botón de **enviar** se debe desactivar si se supera este límite. El usuario debe saber cuántos caracteres quedan mientras esté escribiendo. Si el número de caracteres restantes es inferior a diez, el número que aparece debe cambiar el estilo de presentación para advertir a los usuarios. Debería ser posible borrar el texto del mensaje proporcionado también. Una forma terminada tendria una apariencia similar a la siguiente captura de pantalla:

![chapter-1-4](/img/chapter-1-4.png)

Los requisitos anteriores no son particularmente difíciles y describen una forma de texto bastante estándar. Sin embargo, hay muchos elementos de interfaz de usuario para coordinar aquí, como por ejemplo tenemos que asegurarnos de que, el estado **desactivado** del botón se gestione correctamente, la cantidad de caracteres sea la correcta y que se muestre con un estilo apropiado, y así sucesivamente. El primer intento para lograr nuestra aplicación es la siguiente:

```
<div class="container" ng-controller="TextAreaWithLimitCtrl">
  <div class="row">
    <textarea ng-model="message">{{message}}</textarea>
  </div>
  <div class="row">
    <button ng-click="send()">Send</button>
    <button ng-click="clear()">Clear</button>
  </div>
</div> 
```

Vamos a usar el código anterior como punto de partida y construir sobre él. En primer lugar, tenemos que ver la cantidad de caracteres, lo que es bastante fácil, como se indica en el siguiente código:

```
<span>Remaining: {{remaining()}}</span>
```

La función `remaining()` se define en el controlador `TextAreaWithLimitCtrl` en el `$scope` de la siguiente manera:

```
$scope.remaining = function () {
  return MAX_LEN - $scope.message.length;
};
```

A continuación, tenemos que desactivar el botón **Enviar**, si un mensaje no cumple con las limitaciones de longitud requeridas. Esto se puede hacer fácilmente con un poco de ayuda de la directiva `ng-disabled` de la siguiente manera:

```
<button ng-disabled="!hasValidLength()"...>Send</button>
```

Podemos ver un patrón recurrente aquí. Para manipular la interfaz de usuario, sólo tenemos que tocar una pequeña parte de la plantilla y describir un resultado deseado (Visualización del número de de caracteres restantes, deshabilitar un botón, y así sucesivamente) en términos de estado del modelo (tamaño de un mensaje, en este caso) . Lo interesante aquí es que no es necesario mantener las referencias a elementos DOM en el código JavaScript y no estamos obligados a manipular elementos DOM de forma explícita. En su lugar, podemos simplemente centrarnos en las mutaciones del modelo y dejar AngularJS hacer el trabajo pesado. Todo lo que tenemos que hacer es dar algunas pistas en forma de directivas.

Volviendo a nuestro ejemplo, todavía tenemos que asegurarnos de que el número de caracteres restantes cambie de estilo cuando sólo queden unos pocos caracteres remanentes. Esta es una buena ocasión para ver un ejemplo más de la declarativa interfaz de usuario en  acción, tal como figura en el siguiente código:

```
<span ng-class="{'text-warning' : shouldWarn()}">
  Remaining: {{remaining()}}
</span>
```

Donde el método shouldWarn() es implementado de la siguiente forma: 

$scope.shouldWarn = function () {
  return $scope.remaining() < WARN_THRESHOLD;
};

El cambio de clase CSS está impulsada por la mutación del modelo, ¡pero no hay ninguna lógica de manipulación DOM explícita en cualquier parte del código JavaScript! La interfaz de usuario consigue ser re-pintada basándose en un "deseo" expresado mediante declaración. Lo que queremos decir con la directiva `ng-class` es lo siguiente: "la clase CSS `text-warning` debe añadirse al elemento `<span>`, cada vez que el usuario deba ser advertido, de que se superó el límite de caracteres.” Esto es diferente a decir que “cuando se escribe un nuevo carácter y el número de caracteres excede el límite, quiero encontrar al elemento `<span>` y cambiar la clase CSS `text-warning` del elemento”.  

Lo que estamos discutiendo aquí puede sonar como una diferencia sutil, pero de hecho, los enfoques declarativos e imperativos son bastante opuestos. El estilo imperativo de programación se centra en la descripción de los pasos individuales que conducen a un resultado deseado. Con el enfoque declarativo, el foco se desplaza a el resultado deseado. Los pequeños pasos individuales tomados para llegar a este resultado son atendidos por un marco de apoyo. Es como decir: "Querido AngularJS, así es como quiero que mi interfaz de usuario luzca cuando el modelo termine en un determinado estado. Ahora, por favor ve a averiguar cuándo y cómo repintar la interfaz de usuario".

El estilo declarativo de programación suele ser más expresivo, ya que libera a los desarrolladores de dar instrucciones muy precisas y de bajo nivel. El código resultante es a menudo muy conciso y fácil de leer. Pero por el enfoque declarativo a trabajar, debe haber mecanismos que puedan interpretar correctamente las órdenes superiores. Nuestros programas empiezan a depender de las decisiones de máquinas y tenemos que renunciar a una parte del control de bajo nivel. Con el enfoque imperativo, estamos en control total y se puede ajustar con precisión cada operación individual. Tenemos más control, pero el precio a pagar por "estar a cargo" es un montón de código por escribir repetitivo de bajo nivel.

Personas familiarizadas con el lenguaje SQL encontrarán todo esto suena conocido (SQL es un lenguaje declarativo para consultar data) Simplemente podemos describir el resultado deseado (datos que se encontraron) y dejar una (relacional) base de datos y averiguar cómo hacer para recuperar los datos especificados. La mayoría de las veces, este proceso funciona a la perfección y obtenemos rápidamente lo que hemos solicitado. Sin embargo hay casos en los que es necesario proporcionar pistas adicionales (índices, consejos al planificador de consulta, etc) o tomar el control sobre el proceso de recuperación de datos para ajustar el rendimiento.

Directivas en las plantillas AngularJS declarativamente expresan el efecto deseado, por lo que están libres de proveer instrucciones paso a paso sobre cómo cambiar las propiedades individuales de los elementos DOM (como suele ser el caso en las aplicaciones basadas en jQuery). AngularJS promueve fuertemente el estilo declarativo de programación para las plantillas y un imperativo para el código JavaScript (controladores y lógica de negocio). Con AngularJS, rara vez se aplica, instrucciones imperativas de bajo nivel a la manipulación del DOM (la única excepción es el código de las directivas).

> `TIP` Como regla general, nunca se debe manipular los elementos DOM en los controladores AngularJS. Obtener una referencia de un elemento DOM en un controlador y manipular las propiedades del elemento indica enfoque imperativo en la interfaz de usuario - algo que va en contra del modo que AngularJS construye las interfaces de usuarios.

Las plantillas declarativas de la interfaz de usuario escritas utilizando directivas AngularJs nos permiten describir rápidamente complejas e interactivas interfaces de usuario. AngularJS tomarán todas las decisiones de bajo nivel sobre cuándo y cómo manipular las partes del árbol DOM. La mayoría del tiempo AngularJS hace "lo correcto" y actualiza la interfaz de usuario como se esperaba (y en el momento oportuno). Sin embargo, es importante entender el funcionamiento interno de AngularJS, por lo que podemos ofrecer consejos adecuados al marco si es necesario. Usando la analogía de SQL, una vez más aquí, la mayoría de las veces no es necesario preocuparse por el trabajo realizado por un optimizador de consultas. Pero cuando empezamos a dar con problemas de rendimiento, es bueno saber cómo llegó a sus decisiones el optimizador de consulta para que podamos ofrecer pistas adicionales. Lo mismo ocurre con las interfaces de usuario gestionadas por AngularJS: tenemos que entender los mecanismos subyacentes para usar efectivamente las plantillas y las directivas, hace el código difícil de mantener, probar y leer. 

### Módulos e inyección de dependencias

Los lectores atentos se habrán dado cuenta que todos los ejemplos presentados hasta ahora estaban usando las funciones globales constructoras para definir controladores. Sin embargo, el estado global es malo, duele estructurar la aplicación, hace el código difícil de mantener, probar y leer. De ninguna manera es AngularJS que sugiere el uso de estado global. Por el contrario, viene con un conjunto de APIs que hacen que sea muy fácil definir módulos y registrar objetos en esos módulos.

### Módulos en AngularJS

Vamos a ver cómo convertir una definición de controlador fea globalmente definida en su equivalente modular, anteriormente un controlador era declarado de la siguiente manera:

```
var HelloCtrl = function ($scope) {
  $scope.name = 'World';
}
```

Y al usar los módulos que se ve de la siguiente manera:

```
angular.module('hello', [])
 .controller('HelloCtrl', function($scope){
  $scope.name = 'World';
});
```

AngularJS se define a sí misma como `angular` el espacio de nombre global. Hay varias utilidades y convenientes funciones expuestas en este espacio de nombre, y `module` es una de esas funciones. Un módulo (`module`) actúa como un contenedor para otros objetos gestionados por AngularJS (Controladores, Servicios, etc). Como vamos a ver en breve, hay mucho más que aprender acerca de los módulos que el simple espacio de nombres y organización del código. 

Para definir un nuevo módulo necesitamos proveer un nombre como primer argumento de la función `module`. El segundo argumento hace posible expresar las existencia de dependencias con otros modulos (en el módulo precedente no dependemos de ningún otro módulo).
   
Una llamada a la función `angular.module` retorna una instancia del modulo recien creado. Tan pronto como tenemos acceso a esta instancia, nosotros podemos comenzar a definir nuevos controladores. Es tan sencillo como invocar la función del controlador (`controller`) con los siguiente argumentos: 

- Nombre del controlador (una cadena).
- Función constructora del controlador. 

> `TIP` Las funciones controladoras constructoras definidas globalmente son solamente buenas para ejemplos cortos-rápidos y prototipos rápidos. Nunca uses funciones controladoras globalmente definidas para aplicaciones largas y de producción. 

Ahora un módulo está definido, pero tenemos que informar AngularJS sobre su existencia. Esto se hace proporcionando un valor para el atributo `ng-app` de la siguiente manera:

```
<body ng-app="hello"> 
```

>`TIP` Olvidar especificar el nombre del módulo en el atributo `ng-app` es un error frecuente y una fuente común de confusión. Omitiendo el nombre del módulo en el atributo `ng-app`, dará lugar a un error que indica que el controlador no está definido.

### Objetos Colaboradores

Como podemos ver, AngularJS proporciona una manera de organizar los objetos en módulos. Un módulo puede ser utilizado no sólo para registrar los objetos que se invocan directamente por el marco (controladores, filtros, etcétera) sino para todos los objetos definidos por los desarrolladores de aplicaciones. 

El patrón del módulo es muy útil para organizar nuestro código, pero AngularJS va un paso más allá. Además de registrar los objetos en un espacio de nombres, también es posible describir de forma declarativa dependencias entre los objetos.

### Inyección de dependencias

Ya pudimos ver como el objeto `$scope` estaba siendo misteriosamente inyectado en los controladores. AngularJS de alguna manera es capaz de darse cuenta de que es necesaria una nueva instancia del ámbito en el controlador, AngularJS crea una nueva instancia del ámbito y la inyecta. La única cosa que los controladores tenían que hacer era expresar el hecho de que depende de una instancia `$scope` (no hay necesidad de indicar cómo el nuevo objeto `$scope` debe ser instanciado, o deba ser una instancia recién creada o reutilizada de llamadas anteriores) Toda la gestión de dependencias se reduce a decir algo en este sentido “Para  funcionar correctamente yo necesito una dependencia (un objeto colaborador): yo no sé de donde debería venir o cómo debe ser creado. sólo sé que lo necesito, así que por favor proporcione la dependencia”.

AngularJS tiene internamente un motor de inyección de dependencias (DI). I puede realizar las siguientes actividades.  

- Comprender la necesidad de un colaborador expresada por objetos
- Encontrar un colaborador necesario
- Conectar los objetos conjuntamente en una aplicación completamente funcional
  
La idea de poder expresar declarativamente dependencias es muy poderosa. Libra los objetos de tener que preocuparse de colaborar con los ciclos de vida de otros objetos. Aún mejor, de pronto, es posible intercambiar colaboradores a voluntad, y luego crear diferentes aplicaciones mediante la simple sustitución de determinados servicios. Este es también un elemento clave en la capacidad de hacer pruebas unitarias efectivas en los componentes.  

### Beneficios de la inyección de dependencias 

Para ver todo el potencial de un sistema implementado inyección de dependencias, consideramos un simple ejemplo, un servicio de notificaciones donde más tarde podremos insertar y recuperar mensajes.  Para complicar un poco el escenario, digamos que queremos tener un servicio de archivado. Debería cooperar con nuestro servicio de notificaciones de la siguiente manera, tan pronto como el número de notificaciones exceda cierto umbral, las notificaciones más antiguas deben ser movidas a un archivo. El problema adicional es que queremos ser capaces de utilizar diferentes servicios de archivado en diferentes aplicaciones. A veces el volcado de memoria de los mensajes antiguos por la consola del navegador es todo lo que se necesita, en otras ocasiones nos gustaría enviar notificaciones caducadas a un servidor usando solicitudes XHR. 

El código para el servicio de notificaciones podría tener el siguiente aspecto:

```
var NotificationsService = function () {
this.MAX_LEN = 10;
  this.notificationsArchive = new NotificationsArchive();
  this.notifications = [];
};

NotificationsService.prototype.push = function (notification) {
  var newLen, notificationToArchive;
  newLen = this.notifications.unshift(notification);
  if (newLen > this.MAX_LEN) {
    notificationToArchive = this.notifications.pop();
    this.notificationsArchive.archive(notificationToArchive);
  }
};

NotificationsService.prototype.getCurrent = function () {
  return this.notifications;
};
```

El código anterior está estrechamente acoplado a una implementación del archivo (`NotificationsArchive`), desde que esta particular implementación es instanciada usando la palabra clave `new`. Es una pena, ya que el único contrato en el que ambas clases tienen que cumplir es el método archivar (`archive`) (aceptando un mensaje de notificación para ser archivado).

La capacidad de intercambiar colaboradores es muy importante para verificabilidad.  Es difícil imaginar objetos de prueba de forma aislada y sin la capacidad de sustituir las implementaciones reales por falsos dobles (objetos simulados).  En las siguientes páginas de este capítulo, vamos a ver cómo refactorizar este grupo estrechamente unido de objetos en un conjunto flexible y comprobable de servicios que trabajen juntos. En el proceso de hacer esto, vamos aprovechar al máximo el subsistema de inyección de dependencias de AngularJS.

### Registrando servicios

AngularJs sólo es capaz de cablear objetos de los cuales es consciente, Como consecuencia, el primer paso para conectar el objeto con la maquinaria de inyección de dependencia es registrar el objeto con un módulo AngularJS. No vamos a registrar la instancias de objetos directamente, en lugar de eso vamos a lanzar recetas para crear objetos dentro del sistema de inyección de dependencia. AngularJS interpreta esas recetas para instanciar objetos y a continuación, los conecta en consecuencia. El efecto final es un conjunto de objetos conectados instanciados, que forman una aplicación en ejecución.

En AngularJS hay un servicio dedicado `$provide` que nos permite registrar diferentes recetas para crear objetos. Las recetas registradas son interpretadas por el servicio `$injector` para proveer instancias completas listas para ser usadas (Con todas las dependencias resueltas e inyectadas). 

Los objetos que son creados por el servicio `$injector` se conocen como servicios, AngularJS interpretara la receta dada solo una vez durante el ciclo de vida de la aplicación, y como resultado creara solo una instancia del objeto. 

> `NOTA` los servicios creados por `$injector` son únicos, habrá solo una instancia de un servicio dado por cada instancia de una aplicación en ejecución.

Al final del dia, los modulos AngularJs sólo poseen un conjunto de objetos instanciados pero nosotros podemos tomar el control de cómo esos objetos son creados.

### Valores 

La forma más fácil de contar con AngularJS para administrar un objeto es registrar uno pre-instanciado de la siguiente forma: 

```
var myMod = angular.module('myMod', []);
myMod.value('notificationsArchive', new NotificationsArchive());
```

Cualquier servicio administrado por el mecanismo de inyección de dependencia de AngularJS necesita tener un nombre único (por ejemplo, `notificationsArchive` tal como en el ejemplo precedente). Lo que sigue es una receta para la creación de nuevas instancias.

Los valores de objetos no son particularmente interesantes, ya que el objeto registrado a través de este método no puede depender de otros objetos. Esto no es un gran problema para la instancia `NotificationArchive`, ya que no tienen ninguna dependencia. En la práctica, este método de registro solo funciona con objetos simples (generalmente se expresa como instancias de objetos ya creados o objetos literales)

### Servicios

Nosotros no podemos registrar el servicio `NotificationsService` como un valor de un objeto, ya que nosotros necesitamos expresar una dependencia en un servicio de archivos, La forma simple de registrar una receta para objetos, que dependen de otros objetos, es registrar una función constructora, nosotros podemos hacer esto usando método de servicio de la siguiente forma: 

```
myMod.service('notificationsService', NotificationsService);
```

Donde la función constructora `NotificationsService` puede ahora ser escrita de la siguiente forma: 

```
var NotificationsService = function (notificationsArchive) {
  this.notificationsArchive = notificationsArchive;
};
```

Usando la inyección de dependencia de AngularJs nosotros podemos eliminar el uso de la palabra clave `new` de la función constructora `NoficiationsService`, Ahora este servicio no tiene nada que ver con instancias dependientes y ahora puede aceptar cualquier servicio de archivado. Nuestra simple aplicación es ahora mucho más flexible!

`NOTA` Un servicio es una de esa palabras sobrecargadas que podría significar muchas cosas diferentes. En AngularJs la palabra se puede referir a cualquiera de los dos métodos para registrar funciones constructoras, (tal como se muestra en el ejemplo anterior) o cualquier objeto único que es creado y gestionado por el sistema de inyección de dependencia de AngularJS, independientemente del método de registro usado (esto es lo que la mayoría de las personas quiere decir cuando usan la palabra servicio en el contexto de los modulos AngularJS).

En la práctica el método servicio (`service`) no es usado con frecuencia pero puede ser útil para registrar funciones constructoras pre-existentes, y así hacer que AngularJs administre objetos creados por aquellos construtores. 


### Fábricas

El método `factory` es otra forma de registrar recetas para la creacion de objetos. Es más flexible comparado con el método `service`, ya que nosotros podemos registrar cualquier función creadora de objetos arbitrariamente. Un ejemplo se muestra en el código siguiente. 

```
myMod.factory('notificationsService',function(notificationsArchive){
  
  var MAX_LEN = 10;
  var notifications = [];
  
  return {
    push:function (notification) {
      var notificationToArchive;
      var newLen = notifications.unshift(notification);
      
      //push method can rely on the closure scope now!
      if (newLen > MAX_LEN) {
        notificationToArchive = this.notifications.pop();
        notificationsArchive.archive(notificationToArchive);
      }
    },
    // otros métodos de la clase NotificationsService
  };
```

AngularJS usa la función `factory` suministrada para registrar un objeto devuelto. Puede ser cualquier objeto JavaScript válido, incluyendo objetos `function`. 

El método `factory` es la forma más común de obtener objetos dentro del sistema de inyección de dependencia. Es muy flexible y puede contener lógica de creación sofisticada. Ya que las fábricas son funciones regulares, nosotros podemos también tomar ventaja de un nuevo ámbito para simular variables privadas. Esto es muy útil porque podemos esconder detalles de implementaciones de un servicio dado. En efecto, en el ejemplo anterior podemos mantener el servicio `notificationToArchive`, todos los parámetros de configuración (`MAX_LEN`) y el estado interno (`notifications`) como privados. 

### Constantes 

Nuestro `NotificationsService` es cada vez mejor. A la vez que se desacopla de sus colaboradores y esconde su estado privado. Desafortunadamente, es complicado codificar la configuración de la constante `MAX_LEN`. AngularJS tiene un remedio para esto, es que las constantes pueden ser definidas a nivel del módulo e inyectadas en cualquier objeto colaborador. 

Idealmente, nosotros podemos tener nuestro servicio `NotificationsService` para proveer un valor de configuración de la siguiente manera: 

```
myMod.factory('notificationsService', function (notificationsArchive, MAX_LEN) {
  …
 // la lógica de la creación no cambia
});
```

Entonces suministrar el valor de configuración fuera de `NotificationsService`, a nivel del módulo como se muestra en el siguiente código: 

```
myMod.constant('MAX_LEN', 10);
```

Las constantes son muy útiles para crear servicios que se puedan ser reutilizados a través de muchas aplicaciones diferentes (como clientes del servicio, lo pueden configurar a voluntad). Solo hay una desventaja de utilizar constantes, esta es, tan pronto como un servicio expresa una dependencia en una constante, el valor para esta constante debe ser suministrada, Algunas veces sería bueno tener valores de configuración por defecto y permitir a los clientes cambiarlas solo cuando sea necesario.

### Proveedores  

Todos los métodos de registros describidos hasta ahora son casos especiales de los más genéricos, la última versión de todas ellas es, `provider`. Aquí esta el ejemplo registrando el servicio `notificationsService` como proveedor. 

```
myMod.provider('notificationsService', function () {
 
  var config = {
    maxLen : 10
  };
 
  var notifications = [];

  return {
    setMaxLen : function(maxLen) {
      config.maxLen = maxLen || config.maxLen;
    },
    $get : function(notificationsArchive) {
      return {
        push:function (notification) {
          …
          if (newLen > config.maxLen) {
            …
          }
        },
        // other methods go here
      };
    }
  };

});
```

Primeramente un `provider` es una función que debe retornar un objeto que contenga la propiedad `$get`. La propiedad `$get` mencionada es una función fabricante, que cuando sea invocada debe retornar una instancia de un servicio (`service`) . Nosotros podemos pensar en los proveedores como objetos que estan integrados en funciones fabricantes de la propiedad `$get`.  

A continuación, un objeto retornado por una funcion proveedora puede tener métodos y propiedades adicionales. Aquellas que estén expuestas, es posible establecer un conjunto de opciones antes que el método `$get` (`factory`) sea invocado. En efecto, nosotros podemos todavía establecer la propiedad de configuración `maxLen`, pero no estamos obligados en adelante ha hacerlo.  Por otra parte, es posible tener una lógica de configuración más compleja, ya que nuestros servicios pueden exponer metodos de configuracion y no solo simples valores de configuración. 

### Ciclos de vida de los Modulos

En párrafos anteriores, nosotros pudimos ver que angular soporta varias recetas para la creacion de objetos. Un `provider` es una receta del tipo especial, ya que puede ser ademas configurada antes de producir cualquier instancias de objetos. Para soportar efectivamente los proveedores, AngularJs divide el ciclo de vida de los modulos en dos fases, que son las siguientes.   

- **La fase de configuración**: Esta es la fase donde todas las recetas son coleccionadas y configuradas. 
- **La fase de ejecución**: Esta es la fase donde nosotros podemos ejecutar cualquier lógica después de la creación de instancias.


### La fase de configuración

Los proveedores pueden ser configurados solo durante la primera fase de configuración. Sin duda no tiene sentido cambiar una receta luego de que los objetos estan terminados, ¿no?    Los proveedores pueden ser configurados tal y como se muestra en el siguiente código: 

```
myMod.config(function(notificationsServiceProvider){
  notificationsServiceProvider.setMaxLen(5);
});
```

Lo importante a notar aquí es una dependencia en los objetos `notificationsServiceProvider` con el sufijo `Provider` representan las recetas que estan lista para ser ejecutadas. La fase de configuración nos permite hacer ajustes de último momento en la fórmula de creación de objetos. 

### La fase de ejecución

La fase de ejecución nos permite registrar cualquier trabajo que debería ser ejecutado al inicio de la aplicación. Uno podría pensar en la fase de ejecución como el equivalente del método principal en otros lenguajes de programación. La gran diferencia es que los módulos AngularJS pueden tener múltiples configuraciones y bloques en ejecución. En este sentido, no hay siquiera un solo punto de entrada (una aplicación en ejecución es una verdadera colección de objetos colaboradores).

Para ilustrar como la fase de ejecución puede ser útil, vamos a imaginar que necesitamos mostrar el tiempo de inicio o el tiempo de actividad de las aplicaciones a los usuarios. Para soportar este requerimiento, podemos establecer el tiempo de inicio de la aplicaciones como una propiedad en la instancia  `$rootScope` de la siguiente forma: 

```
angular.module('upTimeApp', []).run(function($rootScope) {
  $rootScope.appStarted = new Date();
});
```

Y luego recuperar el valor desde cualquier plantilla, como se muestra en el siguiente código: 

```
Application started at: {{appStarted}}
```

`TIP` En el ejemplo mostramos el bloque en ejecución en acción donde estamos estableciendo propiedades directamente en la instancia `$rootScope`. Es importante darse cuenta que la instancia `$rootScope` es una variable global y sufre de todos los problemas del ámbito global. La instancia `$rootScope` debería ser usada para definir nuevas propiedades sólo con moderación y sólo para la las propiedades que necesiten ser accesible desde muchas plantillas. 

### Diferentes fases y diferentes métodos de registro 

Vamos a resumir los diferentes métodos para crear objetos y como esos métodos corresponden a las fases del ciclo de vida del módulo:


|                       | ¿Qué se registró?                                     | ¿Es inyectable durante la fase de configuración?  | ¿Es inyectable durante la fase de ejecución?  |
| --------------------- |-------------------------------------------------------| -------------------------------------------------:| ---------------------------------------------:|
| Constante (Constant)  | Valores constantes                                    |                                                si |                                            si |
| Variable              | Valores variables                                     |                                                no |                                            si |
| Servicio (Service)    | Un nuevo objeto creado por una función constructora   |                                                no |                                            si |
| Fabrica (Factory)     | Un nuevo objeto devuelto por una función fabricante   |                                                no |                                            si |
| Proveedor (Provider)  | Un nuevo objeto creado por la función fabricante $get |                                                si |                                            no |




### Modulos que depende de otros modulos

No solo AngularJs hace un excelente trabajo administrando las dependencias de los objetos, también se encarga de las dependencias de los modulos. Nosotros podemos fácilmente agrupar servicios en un módulo, y por lo tanto crear librerías de servicios (potencialmente reutilizables). 

Como por ejemplo, nosotros podemos mover ambas las notificaciones y los servicios de archivado dentro de sus propios modulos (llamados respectivamente `notifications` y `archive`)  de la siguiente forma: 

```
angular.module('application', ['notifications', 'archive'])
```

De esta forma cada servicio (o grupo de servicios relacionados) se pueden combinar en una entidad reusable (un módulo).  Entonces en el módulo más arriba (nivel de aplicación) puede declarar las dependencias necesarias para todos los modulos, para que la aplicación  funcione apropiadamente. 

La habilidad de depender en otros modulos no esta reservado para los módulos superiores. Cada módulo puede expresar dependencias en módulos hijos o dependientes. De esta forma, los módulos puede formar una jerarquía. Entonces, cuando tratemos con los módulos AngularJS necesitaremos pensar en sobre ellos en dos formas distintas, pero relacionadas, jerarquías:  1) jerarquía  de módulos, 2) jerarquía de servicios (como servicios que puedan expresar dependencias en otros servicios, valores, y constantes). 

Los módulos AngularJS pueden depender entre sí y cada módulo puede contener varios servicios. Pero los servicios individuales también pueden depender de otros servicios. Esto plantea varias preguntas interesantes, que son las siguientes:

- ¿Puede un servicio definido en un módulo Angular depender de servicios de otro módulo?. 
- ¿Pueden los servicios definidos en un modulo hijo depender en servicios de un módulo padre, o solo en servicios definidos en modulos hijos?
- ¿Podemos tener modulos privados de servicios visibles solamente en un cierto módulo.?
- ¿Podemos tener varios servicios con el mismo nombre definidos en diferentes modulos?


### Servicios y su visibilidad  a lo largo de los módulos. 

Como era de esperar los servicios definidos en modulos hijos estan disponibles para inyectarlos dentro sevicios en modulos padres, el siguiente código de ejemplo debería hacerlo más claro. 

```
angular.module('app', ['engines'])
  .factory('car', function ($log, dieselEngine) {
      return {
        start: function() {
          $log.info('Starting ' + dieselEngine.type);
        }
      };
  });

angular.module('engines', [])
 .factory('dieselEngine', function () {
  return {
    type: 'diesel'
  };
});
```

Aquí el servicio `car` es definido en el módulo `app`. El módulo app declara una dependencia en el módulo `engines`, donde el servicio `dieselEngine` es definido. No es sorprendente que un servicio `car` pueda ser inyectado con una instancia del módulo `engines`.

Tal vez lo más sorprendente, sea que los servicios definidos en modulos hermanos sean también visibles el uno al otro. Nosotros podemos mover un servicio `car` dentro de otro módulo, y luego cambiar las dependencias del módulo, entonces tal aplicación dependa de ambos modulos `engines` y `cars`, como sigue a continuación: 

```
angular.module('app', ['engines', 'cars'])

angular.module('cars', [])
  .factory('car', function ($log, dieselEngine) {
    return {
      start: function() {
        $log.info('Starting ' + dieselEngine.type);
      }
    };
  });

angular.module('engines', [])
  .factory('dieselEngine', function () {
    return {
      type: 'diesel'
    };
  });
```

En el caso precedente un servicio de `engines` todavía puede ser inyectado en un servicio del modulo `cars` sin ningun problema. 

> `NOTA` Un servicio definido en uno de los módulos de las aplicaciones, es visible para todos los otros modulos. En otras palabras, la jerarquía de los módulos no influye en la visibilidad de los servicios de otros modulos. Cuando AngularJs inicializa una aplicación, combina todos los servicios definidos a lo largo de todos los modulos dentro de una aplicacion, eso es, en el espacio de nombre global. 

Desde AngularJS combina todos los servicios de todos los módulos en uno grande, a nivel de la aplicación en el conjunto de servicios solo puede haber uno y solo un servicio con un nombre dado. Podemos usar esto a nuestro favor en los casos en que queremos depender de un módulo, pero al mismo tiempo reescribir algunos servicios de este módulo. Para ilustrar esto, nosotros podemos redefinir el servicio `dieselEngine` directamente en el modulo `cars` de la siguiente manera: 

```
angular.module('app', ['engines', 'cars'])
  .controller('AppCtrl', function ($scope, car) {
    car.start();
  });

angular.module('cars', [])
  .factory('car', function ($log, dieselEngine) {
    return {
      start: function() {
        $log.info('Starting ' + dieselEngine.type);
      };
    }
  })

  .factory('dieselEngine', function () {
    return {
      type: 'custom diesel'
    };
  });
```

En este caso, el servicio `car` será inyectado con el servicio `dieselEngine` definido en el mismo módulo como la del servicio `car`. A nivel de módulo `car`,  `dieselEngine`, puede reescribir (la sombra) el servicio `dieselEngine` definido bajo el módulo `engines`. 

> `NOTA` Solo puede haber uno y solo un servicio con un nombre en una aplicación angular. Servicios definidos en modulos cercanos al módulo principal en la jerarquía serán rescribidos por aquellos definidos en modulos hijos. 

En la actual versión de AngularJs, todos los servicios definidos en un módulo son visibles para todos los otros modulos. No hay forma de restringir la visibilidad de los servicios a un modulos o sub conjunto de modulos. 

> `NOTA`: Al tiempo de escribir esto, no hay soporte para modulos de servicios privados. 

### Por que usar modulos AngularJs

El hecho es que AngularJS combina todos los servicios para todos los modulos en un espacio de nombres a nivel de aplicación podría ser una sorpresa. y puedas que te preguntes por qué usar módulos. Al final de dia, todos los servicios finalizan en una bolsa enorme, entonces ¿cual es el punto del laborioso trabajo de dividir servicios en modulos individuales?

Los modulos AngularJS pueden ayudarnos a organizar múltiples archivos JavaScript en una aplicación. Hay muchas estrategias para dividir una aplicación en modulos, y nosotros vamos a gastar una porción grande del *Capítulo 2, Construyendo y Probando* discutiendo diferentes enfoques, los pros y los contras. También, una corta y enfocada ayuda para facilitar las pruebas unitarias así como también podremos cargar un conjunto bien identificado de servicios bajo prueba. Una vez de nuevo, en el *Capítulo 2 Construyendo y Probando*, tiene más detalles.  

### AngularJs y el resto del mundo

Escoger un perfecto marco para tu próximo proyecto no es facil, Algunos marcos pueden encajar mejor en ciertos tipos de aplicaciones y experiencia de equipo, preferencias personales, así como también muchos otros factores pueden dictar la decisión final. 

AngularJs será inevitablemente comparado con otros marcos populares JavaScript MV* , Diferentes comparaciones más probablemente producirán resultados diferentes, y diferentes puntos de vistas que impulsará muchas discusiones apasionadas. En lugar de ofrecer reglas estrictas y rápidas o característica para futuras comparaciones, nos gustaría resumir cómo AngularJS es diferente en comparación con otros marcos.

> `NOTA` Si te gustaria ver como el código escrito con AngularJs se compra con el código escrito con otros marcos, vista *TodoMVC* (http://addyosmani.github.com/todomvc). Este es un proyecto donde puedes ver la misma aplicación (lista de cosas por hacer “TODO List”), implementando diferentes marcos JavaScript MV*. Esta es una oportunidad única para comparar enfoques arquitectónicos y la sintaxis del código, su tamaño y legibilidad.

Hay muchas cosas acerca AngularJs que hacen que se destaque entre la multitud. Ya vimos que su enfoque para con las plantillas de la interfaz de usuario es bastante novedosa,  como se menciona en las siguientes características: 

- Actualizaciones automáticas y el enlace de datos de doble via libera a los desarrolladores del tedioso trabajo de desencadenar explícitamente el repintado de la interfaz de usuario. 

- Generando DOM en vivo desde una sintaxis HTML que es usada como lenguaje de plantillas. Más importante, es posible extender el existente vocabulario HTML.  (creado nuevas directivas), y construir interfaces de usuarios usando un nuevo (HTML-based DSL) lenguaje específico de dominio. 

- El enfoque declarativo da lugar a una forma expresiva y muy concisa interfaz de usuario. 

- La excelente maquinaria de plantillas de la interfaz de usuario no coloca ninguna restricción en el código JavaScript (por ejemplo, los modelos y controladores pueden ser creados usando el viejo y simple JavaScript sin llamar las APIs de AngularJS por todas partes) 

AngularJS está abriendo nuevos caminos al traer sólidas prácticas de prueba al mundo JavaScript. El marco en sí está probado a fondo (práctica lo que predica!), pero toda la historia de testabilidad no se detiene aquí, todo el marco y el ecosistema a su alrededor se construyeron con la testabilidad en mente. Que continúa de la siguiente manera:

- El motor de inyección de dependencias permite la testabilidad, ya que toda la aplicación puede estar compuesta por servicios pequeños perfectamente probados.  

- La mayoría de los códigos de ejemplo en la documentación AngularJs esta acompañado de una prueba, es la mejor prueba de que el código escrito en AngularJS es de hecho comprobable.

- El equipo AngularJS creó un excelente ejecutor de pruebas JavaScript llamado Testacular (espectacular ejecutor de pruebas). Testacular da un giro total al flujo de trabajo en las pruebas logrado una experiencia agradable, que es muy útil, rápida, y fiable. Hacer pruebas no siempre es fácil, por lo que es importante contar con herramientas que nos asistan en lugar de resolver en la via.  

Sobre todo, AngularJS hace el desarrollo web divertido otra vez!, Se ocupa de muchos detalles de bajo nivel de el código de las aplicaciones, es extremadamente conciso. No es raro escuchar que volver a escribir una aplicación en AngularJS reduce el código base en general en un factor de cinco o incluso más. Por supuesto, todo depende de la aplicación y del equipo,  pero AngularJS nos permite movernos rápidamente y producir resultados en un abrir y cerrar de ojos.

### JQuery y AngularJS

AngularJS y jQuery forma una interesante relación que necesita una mención especial,  Para empezar, AngularJs contiene, como parte de sus fuentes, un simplificada versión de jQuery, es decir, jqLite. Esto es realmente un pequeño subconjunto de la completa  funcionalidad que ofrece Jquery que se centra en las rutinas de manipulación del DOM. 

> `NOTA` Al integrar jqLite, AngularJs puede trabajar sin ninguna dependencia o cualquiera librería externa. 

Pero AngularJs es un buen ciudadano de la comunidad JavaScript y puede trabajar mano a mano con jQuery. Al detectar jQuery, AngularJS utilizará su funcionalidad para manipular el DOM en lugar de depender de jqLite.

> `NOTA`  Si planeas usar jQuery con angular necesitas incluirlo antes del script AngularJS. 

Las cosas se complican más si se trata de volver a utilizar cualquiera de los componentes de interfaz de usuario de la suite de jQuery UI. Algunos de ellos trabajan muy bien, pero la mayoría de las veces habrá alguna fricción. Es sólo que la filosofía subyacente de dos bibliotecas es tan diferente que casi no podemos esperar ningún tipo de integración sin fisuras. `Capítulo 8, Construir sus propias directivas` mira de cerca a la integración y la creación de widgets de interfaz de usuario que pueden funcionar correctamente en aplicaciones AngularJS.

### Manzanas y naranjas

jQuery y AngularJS pueden cooperar, pero la comparación de los dos es un negocio difícil. En primer lugar, jQuery nació como una librería que simplifica la manipulación DOM, y como tal se centra en atravesar documentos, manejo de eventos, animación, y las interacciones Ajax.

AngularJS, por otra parte, es un marco completo que trata de abordar todos los aspectos de la moderna web 2.0 en el desarrollo de aplicaciones.

Lo más importante es que AngularJS tiene un enfoque completamente diferente para la construcción de la interfaz de usuario, donde la vista especificada mediante declaración es impulsada por cambios en el modelo. Usando jQuery muy a menudo consiste en escribir código DOM-céntrico que se puede ir de las manos como un proyecto crece (tanto en términos de tamaño y la interactividad).

> `TIP` AngularJS esta centrado en el modelo y jQuery esta centrado en el DOM, paradigmas radicalmente diferentes. Desarrolladores webs experimentados en jQuery que son nuevos en AngularJs podrían caer dentro de una trampa al usar AngularJs con los paradigmas de jQuery en mente.  Esto resulta en un código que “pelea con AngularJS” en lugar de desatar todo su potencial. Es por esto que nosotros recomendamos que omita la dependencia jQuery, mientras aprende AngularJS (solo para no tener la tentación de caer de nuevo a los viejos hábitos y aprender a resolver problemas en la manera AngularJS).

AngularJS adopta un enfoque holístico para el desarrollo de modernas aplicaciones web, y trata de hacer de el navegador una mejor plataforma de desarrollo.

# Un vistazo al futuro

AngularJS tiene un verdadero enfoque novedoso para muchos aspectos del desarrollo web, y que podría dar forma a la forma de escribir código para los futuros navegadores. En el momento de escribir esto, hay dos especificaciones interesantes en obras que se basan en ideas similares a las aplicadas en AngularJS.

La especificación Object.observe (http://wiki.ecmascript.org/doku.php?id=harmony:observe) tiene como objetivo la construcción en un navegador la capacidad de seguimiento de los cambios de objetos JavaScript. AngularJS se basa en la comparación del estado del objeto (comprobación sucia) para desencadenar el repintado de la interfaz de usuario. Teniéndolo de forma nativa en el navegador el mecanismo para detectar cambios en el modelo podría mejorar considerablemente el rendimiento de muchos marcos JavaScript MVC, incluyendo AngularJS. De hecho, el equipo AngularJs hizo algunos experimentos usando la especificación Object.observe y vio hasta en un 20 por ciento a 30 por ciento de mejoras en el rendimiento.

La especificacion de componentes Web (http://dvcs.w3.org/hg/webcomponents/raw-file/tip/explainer/index.html) trata de definir los widgets con un nivel de riqueza visual (la cual no es posible solo con CSS), y la facilidad de composición y reutilizar (que no es posible con bibliotecas de scripts de hoy en día).

Ésta no es una meta fácil, pero las directivas AngularJs muestran que es posible definir widgets bien encapsulados y reutilizables.

AngularJS no es sólo un marco innovador para los estándares de hoy en día, sino que también influye en el espacio de desarrollo web de la mañana. El equipo AngularJS trabaja en estrecha colaboración con los autores de las especificaciones mencionadas, por lo que existe la posibilidad de que muchas ideas promovidas por AngularJS hará en partes internas de tu navegador! Podemos esperar que el tiempo dedicado al aprendizaje y jugando con AngularJS dará sus frutos en el futuro.

### Resumen

Hemos cubierto mucho en este capítulo. Nuestro viaje comenzó por familiarizarse con el proyecto AngularJS y la gente que está detrás de él. Hemos aprendido dónde encontrar las fuentes y la documentación de la biblioteca, para que podamos escribir nuestra primera aplicación "Hello World". Es una agradable sorpresa que AngularJS sea muy ligero y fácil de empezar con el.

La mayor parte de este capítulo, aunque fue sobre la construcción de bases sólidas para el resto de este libro vimos cómo trabajar con los controladores, ámbitos y vistas AngularJS, y cómo estos elementos juegan juntos. Una gran parte de este capítulo se dedica a la forma en que los servicios AngularJS se pueden crear en módulos AngularJS y cablearlos usando la inyección de dependencia.

Finalmente, vimos cómo AngularJS se compara con otros frameworks de JavaScript y lo hace que tan especial. Con suerte, ahora está convencido de que el tiempo pasado en el aprendizaje de AngularJS es tiempo bien invertido.

Las vistas, controladores y servicios AngularJs son los básicos ingredientes de cualquiera aplicación AngularJS, por lo que era necesario para obtener una comprensión a fondo de estos temas.  Por suerte, ahora sabemos cómo empezar a crear tanto la capa de servicio y la vista, así que estamos listos para hacer frente a proyectos reales. En el próximo capítulo vamos a preparar una estructura de una aplicación que no es trivial, empezando desde la organización del código y que abarca temas tales como la construcción y las pruebas.