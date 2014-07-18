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

AngularJs también tiene algunos tesoros escondidos, inyección de dependencias y un fuerte enfoque en la verificabilidad. El soporte incorporado para la inyección de dependencias hace que sea fácil de ensamblar aplicaciones web menores, a través de los servicios exhaustivamente probados. El diseño de la estructura y las herramientas a su alrededor promueven prácticas de prueba en cada etapa del proceso de desarrollo.

### Encuentre su camino en el proyecto

AngularJS es relativamente un nuevo actor en la escena de los marcos MVC del lado del cliente, su versión 1.0 fue lanzada en junio de 2012. En realidad, el trabajo en este marco
se inició en 2009 como un proyecto personal de Miško Hevery, un empleado de Google. La idea inicial resultó ser tan buena que, al momento de empezar a escribir, el proyecto fue oficialmente respaldado por Google Inc, y hay todo un equipo a Google trabajando a tiempo completo en el marco.

AngularJS es un proyecto de código abierto alojado en GitHub (https://github.com/angular/angular.js) y licenciado por Google, Inc., en los términos de la licencia MIT.

### La comunidad

Al final del día, ningún proyecto podría sobrevivir sin gente de pie detrás de él.
Afortunadamente, AngularJS tiene una gran comunidad de apoyo. Los siguientes son algunos
de los canales de comunicación donde se puede discutir temas de diseño y solicitar ayuda:

- lista de correo angular@googlegroups.com (grupo Google)
- Google + comunidad en https://plus.google.com/u/0/communities/115368820700870330756
- \# angularjs canal IRC
- [angularjs] tag en http://stackoverflow.com


Los equipos AngularJS se mantienen en contacto con la comunidad mediante el blog (http://blog.angularjs.org/) y al estar presente en los medios de comunicación social,
Google+ (+AngularJS) y Twitter (@angularjs). También hay reuniones de comunidades que se organizan en todo el mundo, si alguna pasa cerca de algún lugar de donde vive, definitivamente vale la pena asistir!

### Recursos de aprendizaje en línea

AngularJS tiene su propia página web (http://www.angularjs.org) donde puede encontrar todo lo que uno esperaría de un marco respetable: resumen conceptuales, tutoriales, guía para el desarrollador, referencia de la API, y así sucesivamente. El código fuente para todas versiones liberadas de AngularJS pueden ser descargados de http://code.angularjs.org.

Las personas en busca de ejemplos de código no se sentirán decepcionados, ya que la documentación de AngularJS en sí tiene un montón de fragmentos de código. Además de esto, podemos ver una Galería de aplicaciones construidas con AngularJS (http://builtwith.angularjs.org). Un canal de YouTube dedicado (http://www.youtube.com/user/angularjs) que tiene grabaciones de muchos eventos pasados, así como algunos videos tutoriales muy útiles.


### Bibliotecas y extensiones

Si bien núcleo AngularJS está lleno de funcionalidades, la comunidad se mantiene activamente agregando nuevas extensiones casi todos los días. Muchos de ellas se listan en una página web dedicada: http://ngmodules.org.


### Herramientas

AngularJS es construido encima del HTML y JavaScript, dos tecnologías que hemos estado utilizando en el desarrollo web desde hace años. Gracias a esto, se pueden seguir usando nuestros editores favoritos e IDEs, extensiones del navegador, etc sin ningún problema. Además, la comunidad AngularJS ha contribuido con varios e interesantes complementos para caja de herramientas HTML/JavaScript ya existente.


### Batarang

Batarang es una extensión de la herramienta de desarrollo Chrome para la inspección de aplicaciones web AngularJS. Batarang es muy útil para visualizar y analizar en tiempo de ejecución características de las aplicaciones AngularJS. Vamos a utilizar batarang ampliamente en este libro, para mirar bajo el capó de una aplicación en ejecución. Batarang se puede instalar desde la tienda web de Chrome (AngularJS batarang) como cualquier otra extensión de Chrome.

### Plunker y jsFiddle

Tanto Plunker (http://plnkr.co) y jsFiddle (http://jsfiddle.net) hacen muy fácil de compartir fragmentos de código en vivo (JavaScript, CSS y HTML). Si bien estas herramientas no están reservadas exclusivamente para el uso con AngularJS, fueron rápidamente adoptadas por la comunidad AngularJS para compartir ejemplos de código pequeños, escenarios para reproducir errores, y así sucesivamente. Plunker merece mención especial, ya que fue escrito en AngularJS, y es una herramienta muy popular en la comunidad.

### Extensiones y plugins IDE

Cada uno de nosotros tiene un IDE favorito o un editor. La buena noticia es que hay plugins/extensiones existentes para varios entornos de desarrollo populares, tales como Sublime Text 2 (https://github.com/angular-ui/AngularJS-sublime-package), productos Jet Brains (http://plugins.jetbrains.com/plugin?pr=idea&pluginId=6971), y así sucesivamente.

## Curso intensivo AngularJS

Ahora que sabemos dónde encontrar la librería y su documentación anexa, podemos empezar a escribir el código para ver realmente AngularJS en acción. Esta sección del libro sienta las bases para los capítulos posteriores por cubrir: plantillas AngularJS, modularidad, y la inyección de dependencia. Esos son los elementos básicos de construcción de cualquier aplicación web AngularJS.

### Hola Mundo - el ejemplo AngularJS

Vamos a echar un vistazo al típico ejemplo "¡Hola, mundo!" escrito en AngularJS para
obtener la primera impresión de la estructura y la sintaxis que emplea.


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

> `NOTA:` La biblioteca AngularJS es una relativamente pequeña versión miniaturizada y comprimida que tiene un tamaño de alrededor de 30 KB. La versión miniaturizada sin compresión gzip tiene un tamaño de alrededor de 80 KB. No requiere ninguna dependencia de terceros.
>
> Para los ejemplos cortos de este libro vamos a utilizar una versión no miniaturizada, amistosa con el desarrollador, alojada en la red de entrega de contenido de Google (CDN). También el código fuente de todas las versiones de AngularJS también puede ser descargado desde http://code.angularjs.org.

Incluir la biblioteca AngularJS no es suficiente para tener un ejemplo en ejecución. Necesitamos arrancar nuestra mini aplicación. La manera más fácil de hacer esto es mediante el uso de un atributo HTML personalizado `ng-app`.

Una inspección más cercana de la etiqueta `<body>` revela otro atributo HTML no estándar: `ng-init`. Podemos utilizar `ng-init` para inicializar modelo antes que la plantilla sea presentada.

El último punto por cubrir es la expresión `{{name}}` la cual simplemente hace visible el valor del modelo. 

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

