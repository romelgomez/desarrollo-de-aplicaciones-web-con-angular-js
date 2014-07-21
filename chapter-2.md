## Capítulo 2.  Construyendo y Probando

El capítulo anterior sirvió como una introducción a AngularJS, en donde se cubre aspectos prácticos del marco, la gente que está detrás de él, y los escenarios de uso básicos. Ahora, nosotros estamos bien preparados para construir una aplicación web completa y más elaborada. El resto del libro se desarrolla en torno a una aplicación de ejemplo que muestra cómo utilizar AngularJS en proyectos de la vida real.

En los siguientes capítulos, vamos a construir una simplificada herramienta de gestión de proyectos de soporte ágil SCRUM, una metodología de desarrollo de software. Esta aplicación de ejemplo nos ayudará a ver como son en AngularJS las APIs, los lenguajes, los escenarios típicos, la comunicación con el servidor, la navegación, la seguridad, la internacionalización, etc. Este capítulo da a conocer la aplicación de ejemplo, el dominio del problema, y la pila de técnicas usadas.    

Normalmente cada proyecto comienza con algunas decisiones iniciales con respecto a la estrategia de organización de archivos, un sistema de construcción, y los básicos flujos de trabajo empleados. Nuestra aplicación de ejemplo no es diferente, por lo que vamos a discutir en este capítulo el sistema de construcción así como también temas relacionados con el diseño del proyecto.     

Las pruebas automatizadas es una práctica sólida de ingeniería que es promovida por AngularJS y todo el ecosistema que lo rodea. Creemos firmemente que las pruebas automatizadas son obligatorias salvo en el caso de los proyectos más triviales. Esta es la razón por la cual la última parte de este capítulo está dedicada a las pruebas, los diferentes tipos de ellas, mecanismos y flujos de trabajo, mejores prácticas y herramientas.

En este capítulo,  vamos a aprender más acerca de:

- La aplicación de ejemplo utilizada en este libro, el dominio del problema, y la pila de técnicas usadas.    

- El sistema recomendado para construir las aplicaciones web AngularJS, así como herramientas y flujos de trabajo asociados. Construyendo y probando.

- La organización sugerida de archivos, carpetas y módulos.

- Las prácticas de pruebas automatizadas, Los diferentes tipos de pruebas y su lugar en el proyecto. Usted se familiarizará con las bibliotecas de pruebas y las herramientas utilizadas habitualmente en aplicaciones web AngularJS.

## Presentamos la aplicación de ejemplo

En esta sección se ofrecen más detalles sobre la aplicación de ejemplo que servirá como un estudio de caso en este libro.

> `NOTA` El código fuente de la aplicación de ejemplo está disponible al público en el repositorio git alojado en GitHub en https://github.com/angular-app/angular-app. Este repositorio contiene el código fuente completo, instrucciones detalladas de instalación y todo el historial del proyecto. 

### Familiarizarse con el dominio del problema. 

Para mostrar AngularJS en su entorno más ventajoso, vamos a construir una herramienta de gestión de proyectos para apoyar a los equipos que utilizan la metodología SCRUM.

> `TIP` AngularJS brilla cuando se utiliza como un marco para la construcción de aplicaciones tipo CRUD, las que consisten en muchas pantallas llenas de formularios dinámicos, listas y tablas.

SCRUM  es un ágil método popular para ejecutar proyectos, así que espero que muchos lectores ya están familiarizados con el. Los nuevos en SCRUM no debe preocuparse, ya que los conceptos básicos de SCRUM son fáciles de entender. Hay muchos excelentes libros y artículos que cubren SCRUM en profundidad, pero para entender lo básico probablemente sea suficiente revisar el artículo introductorio en Wikipedia http://es.wikipedia.org/wiki/Scrum.     

El objetivo de nuestra aplicación de ejemplo es ayudar a los equipos en la gestión de los artefactos SCRUM: proyectos y su atrasos, carreras y su atrasos, tareas, cuadros de progreso, y así sucesivamente. La aplicación también cuenta con un módulo de administración en pleno funcionamiento para administrar usuarios y proyectos que se estén trabajando.

> `NOTA` Nuestra aplicación de ejemplo no se esfuerza en la adhesión plena y estricta a todos los principios SCRUM. Cuando sea necesario tomaremos varios atajos para ilustrar claramente el uso AngularJS a expensas de la construcción de una verdadera herramienta de gestión de proyectos SCRUM.

La aplicación, al final se verá como se muestra en la siguiente captura de pantalla:

![chapter-2-1](/img/chapter-2-1.png)

Viendo la captura de pantalla, podemos ver inmediatamente que lo que vamos a construir es una aplicación bastante típica web CRUD que cubre:

- Recuperar, mostrar y editar datos de un almacén persistente.
- Autenticación y autorización.
- Un esquema de navegación bastante complejo con todos los elementos de apoyo de la interfaz de usuario, tales como menús, pan rallado (breadcrumbs), y así sucesivamente.

### Conjunto de técnicas

Este es un libro centrado en AngularJS, pero para construir cualquier aplicación web nada trivial necesitamos algo más que una librería JavaScript que se ejecuta en el navegador. Como mínimo se requiere un almacén de persistencia y un servidor. Este libro no prescribe ningún conjunto de técnicas y nos damos cuenta de que la gente va a utilizar AngularJS en relación a diferentes servidores, marcos para servidor y almacenes persistentes. Aún así, tuvimos que tomar decisiones para ilustrar cómo AngularJS cabe en un cuadro más grande.

En primer lugar, hemos optado por utilizar las tecnologías JavaScript amigables  siempre que sea posible. Hay muchos excelentes back-ends (servidores), plataformas y almacenes de persistencia por ahí, pero nos gustaría que nuestros ejemplos sean fáciles de seguir para los desarrolladores de JavaScript. Además, estábamos tratando de utilizar las tecnologías que se consideran como la corriente principal en el ecosistema JavaScript.

> `NOTA` Todos los elementos del conjunto y herramientas descritas en este libro están basadas en JavaScript, son de uso gratuito, son proyectos de código abierto.

### Almacén de persistencia 

Cuando se trata de almacenamiento de datos, hay muchas opciones para elegir y el reciente movimiento NoSQL, sólo multiplica el número de alternativas. Para el propósito de este libro vamos a utilizar la base de datos MongoDB orientada a documentos, ya que encaja muy bien en el entorno JavaScript:

- Los documentos son almacenados utilizando el formato de datos JSON (JSON Binario - en resumen BSON).

- Puedes consultar y manipular datos usando JavaScript y JSON. 

- Es posible exponer datos como puntos finales REST para servir y consumir datos en formato JSON.

No se necesita ningún conocimiento previo de MongoDB para seguir los ejemplos de este libro, vamos a tratar de explicar los bits difíciles al examinar los fragmentos de código. Y de ninguna manera estamos sugiriendo que las bases de datos NoSQL, MongoDB, son la mejor opción para las aplicaciones AngularJS. AngularJS es indiferente con el back-end y el almacenamiento. 

### MongoLab

Es relativamente fácil empezar con MongoDB, y la mayoría de los desarrolladores de JavaScript se sentirán como en casa mientras trabajan con esta base de datos orientada a documentos. Para hacer las cosas aún más fáciles nuestra aplicación de ejemplo se basa en la versión hospedada de MongoDB. Como resultado, ninguna instalación de software es necesaria para ejecutar los ejemplos de este libro.

Existen múltiples opciones disponibles de hosting basadas en la nube para MongoDB, pero hemos encontrado que MongoLab (https://mongolab.com) es fiable, es facil de empezar con el y ofrece alojamiento gratuito a base de datos que estén por debajo de los 0.5GB. El espacio de almacenamiento gratuito que ofrece MongoLab es más que suficiente para jugar con los ejemplos de este libro. 

MongoLab tiene una ventaja más que no es despreciable, MongoLab expone la bases de datos de forma ordenada a través de una interfaz muy bien diseñada tipo REST. Vamos a confiar en esta interfaz para demostrar cómo AngularJS puede llamar a los “endpoints” REST.

La inscripción en línea es necesaria para empezar a utilizar MongoLab, es un proceso bastante sencillo que se reduce a rellenar un breve formulario, electrónico. Tan pronto como se hace esto, se puede disfrutar del acceso a la alojada base de datos MongoDB, a la interfaz REST hablando JSON, y a la consola de administración en línea.

### Entorno del lado del servidor

Se puede acceder a las bases de datos hospedadas por MongoLab directamente desde las aplicaciones AngularJS a través de la interfaz REST. Si bien hablar con MongoLab directamente desde una navegador puede ser un buen enfoque para proyectos muy simples, no es una opción que uno elegiría para cualquiera aplicación de la vida real, de cara al público.  Simplemente no hay suficiente seguridad incorporada en la oferta de MongoLab.

En la práctica la mayoría de las interfaces de usuario escritas en AngularJS se comunicará con algún tipo de programa de fondo para recuperar los datos. Un “Middleware” típicamente proporcionará servicios de seguridad (autenticación) y también verificará los derechos de acceso (autorización). Nuestra aplicación de ejemplo no es diferente y necesita un back-end. Una vez más, vamos a apostar por una solución basada en JavaScript; Node.js.

Los binarios node.js están disponibles para todos los sistemas operativos más populares, y se pueden obtener en http://nodejs.org/. Es necesario descargar e instalar Node.js con el fin de ser capaz de ejecutar los ejemplos de este libro en su equipo local. 

> `NOTA` La introducción Node.js está mucho más allá del alcance de este libro. Afortunadamente sólo un conocimiento muy básico de Node.js y de su gestor de paquetes (Node.js gestor de paquetes - npm) es necesario para ejecutar los ejemplos de este libro. Los desarrolladores familiarizados con Node.js les será más fácil entender que está sucediendo en el lado del servidor de la aplicación de ejemplo, pero el conocimiento sobre Node.js no es obligatorio para seguir y entender los ejemplos AngularJS de este libro.

Además del propio Node.js vamos a utilizar las siguientes librerías node.js para construir los componentes de servidor de la aplicación de ejemplo:

- Express (http://expressjs.com/) como el framework de aplicaciones web del lado del servidor que puede proporcionar enrutamiento, servir datos y recursos estáticos.
- Passport (http://passportjs.org/) como un “middleware” de seguridad para Node.js.
- Restler (https://github.com/danwrong/restler) como una biblioteca de cliente HTTP para Node.js.

Si bien familiarizarse con Node.js y las mencionadas bibliotecas podría ser útil, está no es necesaria para aprender efectivamente AngularJs. Es posible que desee profundizar en Node.js y las bibliotecas listadas, si desea obtener un mayor conocimiento de la parte del servidor de los ejemplos que hemos preparado para este libro. Si, por otro lado, su trabajo implica el uso de diferentes back-end puede ignorar la mayor parte de los detalles relacionados con node.js. 

### Librerías JavaScript de terceros 

AngularJS se puede utilizar como una librería independiente para escribir aplicaciones bastantes complejas. Al mismo tiempo, no trata de reinventar la rueda y reimplementar bibliotecas populares.

La aplicación de ejemplo trata de mantener al mínimo las dependencias en librerías externas o de terceros para que podamos centrarnos en lo que AngularJS puede hacer por nosotros. Sin embargo, muchos proyectos usarán librerías populares como jQuery, underscore.js etc. Para demostrar cómo AngularJS puede coexistir con otras librerías la última versión compatible de jQuery esta en el proyecto.  

### Bootstrap CSS

AngularJS no forsa o prescribe cualquier uso particular del CSS por lo que cualquiera es libre de dar el estilo a su aplicación según lo necesite. Para hacer que nuestra aplicación luzca bien nosotros vamos a usar el popular Twitter Bootstrap (http://twitter.github.com/bootstrap/)  CSS.

La aplicación de ejemplo incluye las plantillas LESS de los “Bootstrap's” para ilustrar cómo la compilación de LESS puede ser incluida en la cadena de construcción. 

## Sistema de construcción. 

Hubo momentos en los que se utilizó JavaScript como un lenguaje de juguete, esos días se acabaron hace tiempo. En los últimos años, JavaScript se ha convertido en un lenguaje de corriente principal. Las nuevas aplicaciones, grandes y complejas que se escriben y se despliegan todos los días. El aumento de la complejidad implica que más código está siendo escrito. En estos días no es raro ver proyectos que consisten en decenas de miles de líneas de código JavaScript. 

Ya no es práctico tener un archivo JavaScript que pueda ser simplemente incluido en un documento HTML, necesitamos un sistema de generación. Nuestro archivos JavaScript y CSS se someterán a muchos controles y transformaciones antes de ser desplegado en servidores de producción. Algunos ejemplos de esas transformaciones incluyen:  

- Se debe verificar que el código fuente JavaScript, cumpla con los estándares de código usando herramientas como como jslint (http://www.jslint.com/), jshint (http://www.jshint.com/) o similares. 

- Las suites de prueba deben ser ejecutadas tan a menudo como sea posible, al menos como parte de cada construcción. Por lo tanto este tipo de herramientas y procesos de prueba deben estar bien integrados con el sistema de construcción.

- Algunos archivos pueden necesitar ser generados (por ejemplo, archivos CSS a partir de plantillas como LESS).

- Los archivos deben ser fusionados y miniaturizados para optimizar el rendimiento en el navegador.
  
Además de los pasos que se enumeran anteriormente, por lo general hay otras tareas que se deben ejecutar antes de una aplicación se pueda implementar en servidores de producción, tales como copiar los archivos en su un destino final, actualizar la documentación, y así sucesivamente. En un proyecto no trivial siempre hay muchas tareas laboriosas que debemos tratar de automatizar.

### Construir los principios del sistema

A los desarrolladores les encanta escribir código pero en realidad solo una parte de nuestro tiempo la pasamos frente a un editor de texto. Además de la parte más divertida (diseñó, hablar con los compañeros, codificación y corrección de errores) hay un montón de tareas agotadoras que deben ser ejecutadas a menudo, a veces muy a menudo. Crear una aplicación es una de las tareas que se deben ejecutar una y otra vez por lo que debe ser lo más rápido y menos doloroso posible. A continuación, se discuten algunos de los principios que rigen el diseño de nuestro sistema de construcción.

### Automatizar todo

Los autores de este libro son muy malos ejecutando tareas manuales repetitivas una y otras vez. Somos lentos, comete errores, y para ser sincero, nos aburrimos fácilmente con la tarea de construcción. Si usted es como nosotros usted comprenderá fácilmente por qué creemos firmemente en la automatización de todas las medidas posibles del proceso de construcción. 

Los ordenadores son buenos para hacer el trabajo repetitivo por nosotros, no toman pasos en falso, no es necesario tomar un café y no se distraen. Nuestras máquinas no se quejan si descargamos la mayoría de las tareas recurrentes en ellos! El tiempo invertido en la automatización de todas las medidas posibles del proceso de construcción dará sus frutos rápidamente, ya que se ahorra tiempo todos los días!

### Falla rápido, Libre de fallos. 

Un proceso típico de construcción consistirá en varios pasos distintos. Algunos de esos pasos se ejecutará bien la mayor parte del tiempo, mientras que otros irán fallando de vez en cuando. Las tareas que son más probables a fallar debería ejecutarse al inicio del proceso, y romper la estructura en cuanto se detecta una anormalidad. Esto asegura que no se ejecuten los pasos que consumen más tiempo hasta que los básicos sean cubiertos.

Hemos aprendido de la manera difícil la importancia de esta regla. Inicialmente, el sistema de construcción de la aplicación de ejemplo estaba ejecutando pruebas automatizadas antes de jshint. A menudo nos encontramos esperando por todas las pruebas automatizadas para ejecutar, sólo para encontrar que la estructura se rompió debido al JavaScript inválido. La solución fue trasladar la tarea a jshint desde el inicio de la construcción de tuberías y así romper la estructura lo antes posible.

Es importante incluir mensajes de error claros que se impriman cuando la estructura se rompa. Con mensajes de error sin ambigüedades en su lugar, deberíamos ser capaces de identificar la causa raíz de la falla en cuestión de segundos. No hay nada peor que mirar a un mensaje de error crítico y estar preguntándose qué está pasando cuando ya deberíamos estar yendo a producción con nuestro proyecto.

### Diferentes flujos de trabajo, diferentes comandos. 

Como desarrolladores somos responsables de diferentes tareas, Un dia podriamos estar ocupados añadiendo nuevo código (con pruebas!). y pasando tiempo integrando nuevas características desarrolladas el día anterior. Nuestro sistema de construcción debería reflejar esta realidad suministrado diferentes comandos para diferentes tipos de flujos de trabajo. En nuestra experiencia, un sistema de construcción debe tener tres tareas de compilación diferentes:

1. Rápido para ejecutar y criticar para hacer valer la corrección de código: En un proyecto JavaScript podría significar ejecutar jslint / jshint y ejecutar pruebas unitarias. Este comando debería ser realmente rápido para que pueda ser ejecutado con mucha frecuencia. Esta tarea de construcción es muy útil para el desarrollo de código utilizando el método Test Driven Development (TDD).

2. Un comando para desplegar una aplicación completamente funcional para fines de prueba: Después de ejecutar esta tarea de compilación deberíamos ser capaces de ejecutar una aplicación en un navegador. Para ello, tenemos que hacer más procesamiento aquí, tales como la generación de los archivos CSS, etc. Esta tarea de compilación se centra en un flujo de trabajo de desarrollo relacionados con la interfaz de usuario.

3. Una tarea para desplegar en producción que debería ejecutar todas las verificaciones listadas en los pasos previos como también preparar una aplicación para el despliegue fina, tal como funcionar y minificar los archivos, ejecutar las pruebas de integración, etc.  

### Los scripts compilados también son código   

Scripts compilados son parte de los artefactos del proyecto, y debemos tratarlos como el mismo cuidado que cualquier otro entregable. Scripts compilados tendrán que ser leídos, comprendidos y mantenidos - al igual que cualquier otro tipo de código fuente. Scripts compilados mal escritos pueden ralentizar considerablemente su progreso y garantizar algunas secciones de depuración frustrantes.

### Herramientas

Los principios de diseño del sistema de construcción descrito en la sección anterior pueden ser aplicados a cualquier proyecto y a cualquier conjunto de herramientas. Nuestra seleccionada cadena de herramientas es independiente del sistema operativo, por lo que debes ser capaz de ejecutar aplicaciones de ejemplo en cualquier sistema operativo popular. 

> `NOTA` Aunque hemos seleccionado las herramientas de construcción que, en nuestra opinión, son las mejores para el propósito del proyecto de ejemplo, nos daremos cuenta que en los diferentes proyectos se utilizarán diferentes herramientas. El resto de este libro y recomendaciones de este capítulo todavía deben ser relevantes y fácilmente adaptable a diferentes sistemas de construcción.

### Grunt.js 

El sistema de construcción de la aplicación de ejemplo está impulsado por Grunt (http://gruntjs.com/).  Grunt se promociona como:  

> “Son tareas ejecutadas en la consola, herramientas de construcción para proyectos de JavaScript."

Lo que es importante para nosotros es que grunt.js compila scripts escritos en JavaScript y los ejecuta en la plataforma de node.js. Estas son muy buenas noticias ya que significa que nosotros podemos usar la misma plataforma y el mismo lenguaje de programación para la construcción y ejecución de la aplicación de ejemplo. 

Grunt pertenece a la misma categoría de herramientas de compilación orientadas a tareas como Gradle (scripts escritos en Groovy) o Rake (scripts escritos en Ruby), así que la gente familiarizada con esas herramientas deben sentirse como en casa.  
 
### Librerías y herramientas de prueba

AngularJS usa, promueve y fomenta las prácticas de pruebas automatizadas. El equipo AngularJS toma muy en serio la capacidad de prueba, y se aseguró de que el código escrito utilizando AngularJS sea fácil de probar. Pero toda la historia de la capacidad de prueba no se detiene aquí, ya que el equipo AngularJS escribió o extendido las herramientas con el fin de que el análisis sea fácil en la práctica.

### Jasmine

Las pruebas para el código base de AngularJS fueron escritas usando Jasmine (http://pivotal.github.com/jasmine/). Jasmine es un marco de trabajo para probar código JavaScript y tiene sus raíces en el movimiento Desarrollo Guiado Por Comportamiento (Behavior Driven Development BDD) el cual tiene influencia en su sintaxis. 

Todos los ejemplos de la documentación original de AngularJS estan usando la syntaxis Jasmine, por lo que este marco de pruebas es una opción natural para nuestra aplicación de ejemplo, Por otra parte, AngularJS ha escrito varios objetos ficticios y extensiones Jasmine para proporcionar una experiencia práctica, bien integrada, de las pruebas, dia a dia. 

### Corredor Karma

El corredor karma (http://karma-runner.github.io) esta disponible para ejecutar pruebas JavaScript facilmente. Karma es un corredor que nació para reemplazar otro corredor de pruebas popular TestDriver por una solución JavaScript, estable basada en node.js. 

El corredor Karma es capaz de enviar la fuente y el código de prueba a una instancia en ejecución de un navegador (o empezar una nueva si es necesaria!), activa la ejecución de pruebas, recoge los resultados de las pruebas individuales y reportar el resultado final. Este es un gran negocio en el mundo JavaScript, ya podemos ejecutar simultáneamente pruebas en varios navegadores, asegurándose de que nuestro código funcione correctamente en todo el ecosistema.

> `NOTA` El corredor Karma es una herramienta notable cuando se trata de estabilidad y velocidad. Se utiliza en el proyecto AngularJS para ejecutar pruebas como parte de la continua integración a construir. En cada construcción el corredor Karma ejecuta alrededor de 2.000 pruebas unitarias en varios navegadores. En total, alrededor de 14.000 pruebas se ejecutan en unos 20 segundos. Esas cifras deberían aumentar nuestra confianza en el corredor Karma como herramienta y en AngularJS como marco.

La prueba es un tema tan central en AngularJS que vamos a tener una mirada más profunda en las pruebas Jazmín y en el corredor Karma más adelante en este capítulo.

## Organización de archivos y carpetas

Hasta ahora hemos tomado algunas decisiones muy importantes con respecto a la pila de técnicas y herramientas a utilizar, mientras se construye la aplicación de ejemplo SCRUM. Ahora, tenemos que responder a una pregunta más fundamental, cómo organizar los diferentes archivos en una estructura de carpetas significativa. 

Hay múltiples formas de organización de archivos en un determinado proyecto. A veces toman decisiones por nosotros, ya que algunos de los instrumentos y marcos existentes obligan a un diseño establecido. Pero ni grunt.js o AngularJS impone ninguna estructura de directorios en particular por lo que somos libres de tomar nuestras decisiones. En los párrafos siguientes se encuentra una propuesta de organización de los archivos y los fundamentos detrás de esta propuesta. Usted puede optar por utilizar la estructura descrita como tal en sus proyectos futuros o ajustar de acuerdo con sus necesidades particulares.

### Carpetas raíz

Si bien en el diseño de las carpetas a nosotros nos gustaría terminar con una estructura de directorios que nos haga fácil navegar en el código base.  Al mismo tiempo tenemos que mantener una estructura compleja a un nivel razonable. 

Estas son algunas de las suposiciones básicas que nos conducen a la estructura de directorios de alto nivel:

- El código fuente de la aplicación y de las pruebas complementarias deben estar claramente separados. Esto es para mantener el sistema fácil de mantener, ya que por lo general hay diferentes conjuntos de tareas para ser ejecutados, para las pruebas y para el código fuente. 

- El Código de terceros de cualquiera librería externa debería estar claramente aislado de nuestro código base. Las bibliotecas externas cambian a diferentes ritmos, en comparación con nuestras entregas. Queremos que sea fácil actualizar las dependencias externas en cualquier momento. Mezclando nuestras fuentes con librerías externas haría que dichas actualizaciones sean más difícil y que consuma mucho tiempo.   

- Scripts relacionados deben residir en sus propias carpetas específicas y no dispersos a lo largo del código base.

- Los resultados compilados debe salir en una carpeta separada. El contenido y la estructura de la salida compilada debería asemejarse a los requerimientos encargados por la implementación en producción. Debería ser muy fácil sólo tienes que tomar la salida de la compilación e implementarlo en su destino final.   

Teniendo en cuenta todos los supuestos anteriores terminamos con el siguiente conjunto de directorios de nivel superior en el proyecto:

- src:  Contiene el código fuente de la aplicación.
- test: Contiene las pruebas complementarias automatizadas. 
- vendor: Contiene las dependencias de terceros.
- build:  Contiene los scripts de creación. 
- dist: Contiene los resultados compilados, listos para implementar.

Finalmente, aquí se visualiza la estructura de carpetas de nivel superior: 

![chapter-2-2](/img/chapter-2-2.png)

Además de las carpetas descritas podemos notar varios archivos de nivel superior. Sólo como referencia, los archivos son:

- .gitignore:  Son reglas relacionadas con git SCM para indicar que archivos no deben ser rastreados por el repositorio git. 

- LICENSE: Contiene los términos de la licencia MIT. 

- Gruntfile.js: Es el punto de entrada de la estructura grunt.js.

- package.json:  Contiene la descripción del paquete para las aplicaciones node.js 

### Dentro del la carpeta del código fuente de la aplicación

Con los fundamentos abordados, ahora estamos listos para sumergirnos en la estructura de la carpeta `src`. Vamos a echar un vistazo a su estructura en primer lugar:

![chapter-2-3](/img/chapter-2-3.png)

El archivo index.html, es el punto de entrada a la aplicación de ejemplo. A continuación, hay cuatro carpetas, dos de ellas tienen código AngularJS (app y common) y las otras dos contienen objetos de presentación indiferentes a AngularJS (assets and less).

Es fácil descifrar el propósito de las carpetas assets y less, la primera contiene las imágenes y los iconos, mientras que la segunda, las variables LESS. Tenga en cuenta que las plantillas de LESS para Twitter Bootstrap CSS se encuentran en la carpeta del proveedor, aquí  simplemente mantenemos los valores de las variables.

### Archivos específicos de AngularJS

Las aplicaciones AngularJS consisten en dos diferentes tipos de archivos, es decir, scripts y plantillas HTML. Cualquier proyecto excepcional producirá muchos archivos de cada tipo y nosotros necesitamos encontrar una manera de organizar esta cantidad de archivos. A nosotros nos gustaría idealmente agrupar los archivos relacionados y mantener los no relacionados aparte. El problema es que cada archivo puede estar relacionado con cualquier otro en muchas diferentes formas y nosotros tenemos solo un directorio para expresar esa relaciones.      

Las estrategias comunes para resolver este problema involucra agrupar los archivos por función (empacar por función), por capa arquitectónica (empacar por capa), por tipo de archivo. Lo que nos gustaría proponer aquí es un enfoque híbrido. 

- La mayoría de los archivos de la aplicación debe ser organizados por función. Los scripts y parciales que están funcionalmente relacionados entre sí, deben ir juntos. Esta disposición es muy conveniente mientras trabaja en las partes de la aplicación, ya que todos los archivos que cambian conjuntamente estarán agrupados.

- Archivos encapsulados tales como (acceso persistente al almacén de datos, localización, directivas comunes, etc) deben ser agrupados juntos. La relación aquí es que la infraestructura de los scripts no cambia al mismo paso que el resto del código. En el típico ciclo de vida de una aplicación, a medida que la aplicación madura, parte de la infraestructura técnica es escrita al principio y el foco es desplazado al resto del código funcional. Los archivos en común, a nivel de la infraestructura están mejor organizados por capa arquitectónica.   

Nosotros directamente podemos traducir las anteriores recomendaciones en la siguiente estructura de directorios de la aplicación de ejemplo: 

![chapter-2-4](/img/chapter-2-4.png)

> `NOTA` La estructura del directorio descrita aquí, es diferente a la recomendada por el proyecto semilla de AngularJS. (https://github.com/angular/angular-seed). La estructura del directorio del proyecto angular-seed funciona bien para un proyecto simple, pero el consenso de la comunidad AngularJS concluye que en los proyectos grandes, la estructura del directorio están mejor organizada según la función. 

### Empieza simple

Mirando la estructura de carpetas se dará cuenta que algunas de las carpetas que contienen código tienen una jerarquía profunda. Esas carpetas son muy parecidas a la navegación jerárquica en la propia aplicación. Esto es deseable, ya que mirando la interfaz de usuario de la aplicación podemos comprender rápidamente dónde se almacena el código correspondiente.

Es una buena idea iniciar un proyecto con una estructura muy simple, y dar pequeños pasos hacia la estructura final del directorio. Por ejemplo, al inicio, la aplicación de ejemplo, la carpeta admin no tiene subcarpetas, el directorio admin contiene toda la funcionalidad para la gestión de proyectos SCRUM y usuarios. Se añadieron nuevas subcarpetas a medida que el código evolucionó y los archivos fueron cada vez más grandes (y más numerosos). Varias veces la estructura de las carpetas pueden evolucionar, exactamente igual que el código fuente.

### Los Controladores y parciales evolucionan juntos

Es bastante común ver proyectos donde los archivos se organizan en carpetas en función a su tipo. En el contexto AngularJS los script JavaScript y plantillas a menudos son separados en diferentes directorios. Esta separación suena como una buena idea, pero en la práctica las plantillas y los controladores correspondientes tienden a evolucionar al mismo ritmo. Por eso, en el ejemplo las plantillas de la aplicación SCRUM y los controladores se mantienen juntos. Cada área funcional tendrá su propia carpeta y ambas plantillas y controladores residirá en una carpeta.

A modo de ejemplo aquí esta el contenido de la carpeta relacionada con la funcionalidad de administración de los usuarios:

![chapter-2-5](/img/chapter-2-5.png)

### Dentro de la carpeta de pruebas

Las pruebas automatizadas se escriben para hacer valer si una aplicación está funcionando correctamente, y el código de prueba está estrechamente relacionado con el código funcional. Por lo tanto, no debería sorprender que la estructura de la carpeta de prueba imita la del src/app:

![chapter-2-6](/img/chapter-2-6.png)

Es fácil ver que el contenido de la carpeta de prueba casi refleja la carpeta principal. Todas las librerías de pruebas se encuentran en una carpeta dedicada y la configuración del corredor de pruebas Karma también tiene su casa también (config). 

### Convenciones de los nombres de los archivos

Es importante establecer ciertas convenciones para los nombres de los archivos para que la navegación en el código base sea más fácil.  Aquí un conjunto de convenciones que a menudo son seguidas por la comunidad AngularJS y que son adoptadas en este libro.   

- Todos los archivos JavaScripts son nombrados con la extensión estándar .js 

- Los parciales obtiene el sufijo .tpl.html, para distinguirlos fácilmente de otros archivos HTML.  

- Los archivos de pruebas reciben el mismo nombre que el archivo que prueban, y el sufijo depende del tipo de prueba. Las pruebas unitarias tendrán el sufijo .spec.js 

## Archivos y Módulos AngularJS 

Ahora que nuestra aplicación está muy bien organizada en carpetas y archivos, podemos empezar a buscar en el contenido de los archivos.  Aquí nos vamos a centrar en los archivos JavaScript, su contenido y su relación con los módulos AngularJS.

### Un archivo, un módulo. 

En el *Capítulo 1, Angular Zen*, hemos visto que los módulos AngularJS pueden depender unos de otros. Esto significa que en la aplicación AngularJS necesitamos hacer frente tanto a la jerarquía de directorios y como a la jerarquía módulos. Ahora vamos a profundizar más en estos temas con el fin de presentar recomendaciones pragmáticas para la organización de módulos AngularJS y su contenido.

Hay básicamente tres enfoques que podríamos tomar para relacionar los archivos individuales y módulos AngularJS:

- Permitir múltiples módulos AngularJS en un archivo JavaScript
- Tener módulos AngularJS que abarquen varios archivos JavaScript
- Definir exactamente un módulo AngularJS por archivo JavaScript

La definición de múltiples módulos en un archivo no es muy perjudicial, pero puede dar lugar a grandes archivos con cientos de líneas de código. Además de esto, es más difícil encontrar un módulo en el código fuente, como también localizar ambos, el archivo en el sistema de archivos, y el módulo dentro del archivo. Si bien tener múltiples módulos en un solo archivo podría funcionar para proyectos muy simples, no es óptimo para proyectos grandes.

Tener un módulo que abarque varios archivos es algo que debe ser probablemente evitado. Tan pronto como el código de un módulo se distribuye en varios archivos tenemos que empezar a pensar en el orden de carga de los archivos: la declaración del módulo debe venir antes de que se registren los proveedores. Además, tales módulos tienden a ser más grande y, como tal, más difícil de mantener. Los módulos grandes pueden ser especialmente nocivos para las pruebas unitarias, donde queremos cargar y ejercer unidades de código tan pequeñas como sea posible.

De las tres propuestas que tenemos, exactamente un módulo AngularJS por archivo parece ser el enfoque más sensato.

> `TIP`  Mantén los módulos AngularJS pequeños y enfocados al implementar uno por archivo. Además no estará preocupado por el orden de carga de los archivos. También será posible cargar módulos individuales bajo pruebas unitarias.

### Dentro de un módulo 

Tan pronto como un módulo es declarado, este puede ser utilizado para registrar recetas para crear objetos. Como recordatorio, las recetas se denominan proveedores en la  terminología AngularJS. Cada proveedor, cuando se ejecuta, producirá exactamente una instancia del servicio en tiempo de ejecución. Si bien las recetas para la creación de servicios se pueden expresar usando multitud de formas (fábricas, servicios, proveedores, y variables) el efecto neto es siempre el mismo; una instancia configurada de un servicio.

### Diferentes sintaxis para registrar proveedores 

Para registrar un nuevo proveedor nosotros necesitamos la instancia de un módulo. Esta instancia siempre la tendremos al llamar al método `angular.module`.

Nosotros podemos guardar una referencia de la instancia devuelta por el método para reusarla y registrar múltiples proveedores. Por ejemplo, para registrar dos controladores en el módulo responsable de administrar proyectos, podríamos escribir:      

```
var adminProjects = angular.module('admin-projects', []);

adminProjects.controller('ProjectsListCtrl', function($scope) {
 //controller's code go here
});

adminProjects.controller('ProjectsEditCtrl', function($scope) {
 //controller's code go here
}); 
```

Si bien este enfoque ciertamente funciona, tiene un inconveniente, nos vemos obligados a declarar una variable intermediaria (`adminProjects` en este caso) sólo para estar en condiciones de declarar múltiples proveedores en un módulo. Peor aún, la variable intermediaria podría terminar en el espacio de nombres global si no se toman precauciones adicionales (envolver la declaración del módulo en un cierre, la creación de un espacio de nombres, etc.). Idealmente, nos gustaría ser capaces de recuperar de alguna manera una instancia de un módulo ya declarado. Resulta que podemos hacer esto como se muestra en el siguiente ejemplo:

```
angular.module('admin-projects', []);

angular.module('admin-projects').controller('ProjectsListCtrl', function($scope) {
//controller's code goes here
});

angular.module('admin-projects').controller('ProjectsEditCtrl', function($scope) {
//controller's code goes here
});
```

Nos las hemos arreglado para eliminar la variable extra, pero nuestro código no consiguió estar nada mejor. ¿Puedes notar cómo se repite `angular.module('admin-proyectos')` por todo el lugar, toda duplicación de código es mala y este código nos puede golpear duro si decidimos cambiar el nombre de un módulo un día. Además de esto la sintaxis para declarar un nuevo módulo y para recuperar uno existente puede ser fácilmente confundida con el módulo principal, resultados muy confusos. Basta con comparar la expresión: `angular.module('myModule')` con `angular.module('myModule', [])`. Es fácil pasar por alto la diferencia, ¿no es así?

>`TIP` Es mejor evitar la recuperación de módulos AngularJS utilizando el constructor `angular.module('myModule')`. La sintaxis es prolija y resulta en duplicación de código. Peor aún, la declaración del módulo se puede confundir fácilmente con el acceso a las instancias de un módulo existente.

Por suerte hay un enfoque más que aborda todos los problemas descritos hasta ahora. Vamos a echar un vistazo al código en primer lugar:

```
angular.module('admin-projects', [])
  
  .controller('ProjectsListCtrl', function($scope) {
    //controller's code go here
  })
  
  .controller('ProjectsEditCtrl', function($scope) {
    //controller's code go here
  });
```

Podemos ver que es posible encadenar controladores. En cada llamada al método controlador `controller` retornarán una instancia del módulo en el método donde fue invocado. Otros métodos proveedores de servicios como `provider`, `factory`, `service`, `value`, etc, retornarán una instancia del módulo, por lo que es posible registrar diferentes proveedores usando el mismo patrón. 

En nuestra aplicación de ejemplo SCRUM vamos a usar la sintaxis de cadena que acabamos de describir para registrar todos los proveedores. Esta forma de registrar proveedores elimina el riesgo de crear variables innecesarias (potencialmente globales) y evitamos la duplicación de código. También es muy fácil de leer siempre que se coloque un poco de esfuerzo en el formato del código.

### Sintaxis para declarar la configuración y bloques de ejecución 

Como se describe en el `Capítulo 1 AngularJS Zen`, el proceso de inicializar una aplicación AngularJS se divide en dos fases distintas, la fase de configuración y la fase de ejecución. Cada módulo puede tener múltiples configuraciones y bloques de ejecución. Nosotros no estamos restringidos a sólo una. 

Resulta que AngularJS soporta dos formas diferentes de registrar funciones que deben ser ejecutadas en la fase de configuración. Ya hemos visto que es posible especificar una función de configuración en el tercer argumento de la función `angular.module`:

```
angular.module('admin-projects', [], function() {
 //configuration logic goes here
});
```

El método anterior nos permite registrar un único bloque de configuración. Encima de esto la declaración del módulo comienza a ser larga (especialmente, si el módulo específica dependencia en otros módulos.). Como alternativa existe esta forma, el metodo `angular.config`:  

```
angular.module('admin-projects', [])
  .config(function() {
    //configuration block 1
  })

  .config(function() {
    //configuration block 2
  });
```

Como puede ver, el método alternativo permite registrar varios bloques de configuración. Esto podría ser muy útil para dividir las grandes funciones de configuración (especialmente aquellas con múltiples dependencias) en funciones más pequeñas y enfocadas. Las funciones pequeñas y cohesivas son más fáciles de leer, mantener y probar.

En la aplicación de ejemplo SCRUM vamos a utilizar el formulario después de registrar la configuración y ejecutar funciones que nos resulten más fácil de leer.

> `TIP`  Descargar el código de ejemplo. Puedes descargarte el código de ejemplo para todos libros Packt que has comprado desde http://www.packtpub.com, Si compraste este libro en otro lugar, vista y registrate en http://www.packtpub.com/supportand para recivir los archivos directamente en tu correo.  

## Pruebas automatizadas 

Desarrollar software es complicado, como desarrolladores, nosotros necesitamos hacer malabares con los requisitos del cliente y con la presión de las fechas límite, trabajar efectivamente con otros miembros del equipo, y todo esto mientras empiezas a dominar una multitud de herramientas de desarrollo y lenguajes de programación. Como la complejidad del software aumenta también lo hace el número de errores. Todos somos humanos y nosotros cometemos equivocaciones, toneladas de errores. A bajo nivel, los errores de programación son sólo un tipo de defecto con el cual tenemos que luchar constantemente. Instalación, configuración y problemas de integración también son una plaga en cualquier proyecto no trivial. 

Ya que, es tan fácil introducir diferentes tipos de errores en un proyecto, nosotros necesitamos herramientas efectivas y técnicas para dar batalla a esos errores. La industria de desarrollo de software como todo el conjunto se da cuenta cada vez más que las pruebas automatizadas aplicadas rigurosamente pueden garantizar la calidad, y entregas a tiempo. 

Prácticas de pruebas automatizadas en primer lugar fueron popularizados por las metodologías de desarrollo ágil (eXtreme Programming, XP). Pero, lo que era hace varios años nuevo y revolucionario (y hasta cierto punto controversial) es ahora una práctica ampliamente aceptada, una práctica estándar. En estos días, realmente no hay excusa para no tener una batería completa de pruebas automatizadas.

Las pruebas unitarias son nuestra primera línea de defensa en la lucha contra errores de programación. Esas pruebas como su nombre lo indica se centra en pequeñas unidades de código, suelen ser clases individuales o grupos de objetos que colaboran estrechamente. Las pruebas unitarias son una herramienta para un desarrollador. Nos ayudan a hacer valer que tenemos la construcción de bajo nivel correcta, y que nuestras funciones producen los resultados esperados. Sin embargo, las pruebas unitarias traen mucho más beneficios más allá de la verificación de código simple, de una sola vez:

- Ellas nos ayudan a encontrar los problemas temprano en el proceso de desarrollo cuando la depuración y correcciones es menos costosa.  

- Un conjunto bien escrito de pruebas se pueden ejecutar en cada generación  proporcionando un conjunto de pruebas de no regresión. Esto nos da tranquilidad y nos permite introducir cambios en el código con gran confianza.

- Sin duda no hay mejor entorno de desarrollo ligero que un editor y un corredor de prueba. Podemos actuar con rapidez sin necesidad de generar, implementar y hacer clic a través de la aplicación para comprobar que el cambio es correcto.

- Escribir código de prueba por primera vez usando el enfoque Test Drive Development (TDD) nos ayuda a diseñar clases y sus interfaces. Las pruebas sirven como otro cliente que invoca nuestro código y como tal, nos ayuda a diseñar flexibles, interfaces y clases débilmente acopladas.

- Por último, pero no menos importante, existen pruebas que pueden servir como documentación. Nosotros podemos siempre abrir una prueba para ver como el método debe ser invocado, y que conjunto de argumentos acepta. Es más, esta documentación es ejecutable y siempre esta actualizada.  

Las pruebas unitarias son de gran ayuda pero no pueden capturar todos los problemas posibles. En particular todos los problemas de configuración y integración no serán detectados por las pruebas unitarias. Además para cubrir completamente nuestras aplicaciones con pruebas automatizadas nosotros necesitamos ejecutar pruebas de principio a fin (integración) al más alto nivel. La meta de la pruebas automatizadas es ejercitar una aplicación ensamblada y asegurarse que todas las pruebas unitarias trabajen bien juntas para formar una completa aplicación funcional. 

Escribir y mantener pruebas automatizadas es una habilidad aparte, como cualquiera otra habilidad esta necesita ser aprendida y perfeccionada. Al inicio de su jornada de pruebas tal vez sienta que las prácticas de pruebas lo están desacelerando y no valen la pena. Pero a medida que obtenga más competencia con las técnicas de pruebas usted podrá apreciar la cantidad de tiempo que podemos ganar probando rigurosamente el código. 

> `NOTA` Hay una cita que dice que escribir código sin un sistema de control de versiones (VCS) es como el paracaidismo sin paracaídas. Hoy en día uno puede apenas considerar ejecutar un proyecto sin la necesidad de utilizar un VCS. Lo mismo se aplica a la pruebas automática y nosotros podríamos decir: "Escribir software sin un conjunto de pruebas automatizadas es como escalar sin cuerda o el paracaidismo sin paracaídas". Usted puede intentarlo pero los resultados serán casi con seguridad desastrosos.

El equipo AngularJS reconoce completamente la importancia de las pruebas automatizadas y aplica estrictamente prácticas de pruebas mientras trabaja en el marco de trabajo.  Lo que es fantástico, sin embargo, es que AngularJS viene con un conjunto de herramientas, bibliotecas y recomendaciones que hacen que nuestra aplicación sea fácil de probar.

### Pruebas unitarias 

AngularJS adoptó a Jasmine como marco de prueba:  las pruebas unitarias para el marco son escritas usando Jasmine y todos los ejemplos de la documentación también usan la sintaxis Jasmine. Además AngularJS extendió la librería Jasmine original con unos cuantos complementos útiles para hacer las pruebas aún más fáciles.   

### Anatomía de las pruebas Jasmine

Jasmine se promociona a sí misma como el “marco de desarrollo de comportamiento dirigido para probar código JavaScript”. La raíz de las pruebas de comportamiento dirigido están reflejadas en la sintaxis empleada por las especificaciones de la librería que se supone se lean como frases en inglés. Vamos a examinar un ejercicio una simple prueba en una clase standard JavaScript para ver cómo funciona en la práctica:

```
describe('hello World test', function () {
 
  var greeter;
  beforeEach(function () {
    greeter = new Greeter();
  });

  it('should say hello to the World', function () {
    expect(greeter.say('World')).toEqual('Hello, World!');
  });

});
```

Hay varias construcciones en la prueba anterior que necesita unas palabras para explicarlas: 

- La función `describe`, especifica una característica de una aplicación. Actúa como un contenedor de agregación para pruebas individuales. Si usted esta familiarizado con otros marcos de pruebas usted puede pensar en los bloques `describe` como una suites de pruebas. 
Los bloques `describe` pueden ser anidados uno dentro del otro.

- La prueba en sí se encuentra dentro de la función `it`. La idea es ejercitar un aspecto particular de una característica en una sola prueba. Una prueba tiene un nombre y un cuerpo. Por lo general, la primera sección del cuerpo de la prueba se llama a los métodos de un objeto bajo prueba, mientras que el posterior verifica los resultados esperados.

- Código contenido en el bloque `beforeEach` será ejecutado antes de cada prueba individual. Este es un lugar perfecto para cualquier lógica de inicialización que se deba ejecutar antes de cada prueba.

Las últimas cosas a mencionar son las funciones `expect` y `toEqual`. Usando estas dos construcciones nostros podemos comparar los actuales resultados con los esperados.  Jasmine, como muchos otros populares marcos de pruebas, viene con un rico conjunto de comparadores, `toBeTruthy`, `toBeDefined`, `toContain` son solo algunos ejemplos de lo que está disponible.  

### Probando objetos AngularJS

No hay demasiada diferencia al probar objetos AngularJS de otra clase regular JavaScript. El sistema de inyección de dependencia y la forma en que se puede aprovechar  las pruebas unitarias, son específicas de AngularJS. Para aprender como escribir pruebas aprovechando la maquinaria de inyección de dependencia nosotros vamos enfocarnos en probar servicios y controladores. 

> `NOTA` Todas las extensiones de las pruebas relacionadas y los objetos ficticios están distribuidas en un archivo AngularJS llamado angular-mocks.js. No se olvide de incluir este script en el corredor de prueba. Al mismo tiempo, es importante no incluir este script en la versión implementada de la aplicación.

### Probando Servicios 

Escribir pruebas para los objetos registrados en los módulos AngularJS es fácil pero requiere un poco de configuración inicial. Más específicamente, nosotros necesitamos asegurarnos que el módulo AngularJS apropiado esté inicializado y toda la maquinaria de  inyección de dependencia cobre vida. Afortunadamente AngularJS provee un conjunto de métodos que hace que las pruebas Jasmine y inyección de dependencia jueguen juntos muy bien.

Vamos a analizar una simple prueba para el servicio `notificationsArchive` introducido en el *Capítulo 1 Angular Zen*, para ver cómo probar los servicios AngularJS. Como recordatorio aqui esta el codigo para el servicio en sí: 

```
angular.module('archive', [])
.factory('notificationsArchive', function () {
  var archivedNotifications = [];
  return {
    archive:function (notification) {
      archivedNotifications.push(notification);
    },
    getArchived:function () {
      return archivedNotifications;
    }
  };
}); 
````

Aqui esta la prueba correspondiente: 

```
describe('notifications archive tests', function () {
 
  var notificationsArchive;
    
  beforeEach(module('archive'));

  beforeEach(inject(function (_notificationsArchive_) {
    notificationsArchive = _notificationsArchive_;
  }));
 
  it('should give access to the archived items', function () {
    var notification = {msg: 'Old message.'};
    notificationsArchive.archive(notification);
    expect(notificationsArchive.getArchived()).toContain(notification);
  });

});
```

En este punto usted debería reconocer la estructura familiar de prueba Jasmine, además de notar las nuevas funciones: `module` y `inject`.  

La función `module` es usada en las pruebas Jasmine para indicar que los servicios de un módulo dado deben estar preparados para la prueba. El papel de este método es similar a el que desempeña la directiva `ng-app`. El indica que el `$injector` AngularJS debe ser creado por un módulo dado (y todos los módulos dependientes).

> `NOTA` No hay que confundir la función `module` usada en la prueba con el método `angular.module`. Aunque ambas tienen el mismo nombre, sus funciones son muy diferentes. El método `angular.module` se utiliza para declarar nuevos módulos, mientras que la función module nos permite especificar los módulos que se utilizarán en la prueba.
 
En realidad es posible tener múltiples solicitudes a funciones `module` en una prueba. En este caso todos los servicios, valores y constantes de módulos específicos estarán disponible a través de el `$injector`

La función `inject` tiene una simple responsabilidad, esa es inyectar los servicios dentro de nuestras pruebas. 

La última parte puede ser algo confusa, la presencia de los misteriosos subrayados en la llamada de la función inject:

```  
var notificationsArchive;
beforeEach(inject(function (_notificationsArchive_) {
  notificationsArchive = _notificationsArchive_;
}));
```

Lo que está pasando aquí es que el `$injector` va a remover cualquier subrayado principal y posterior al inspeccionar los argumentos de la función para recuperar dependencias. Este es un truco útil ya que podemos guardar los nombres de variables sin guiones para la prueba en sí.


### Probando controladores

Una prueba para un controlador sigue un patrón similar a al de un servicio. Vamos a echar un vistazo a el fragmento del controlador `ProjectsEditCtrl` de la aplicación de ejemplo. Este controlador en cuestión es responsable de los proyectos de edición en la parte de administración de la aplicación. Aquí vamos a probar métodos del controlador responsables de añadir y eliminar miembros del equipo del proyecto:

```
angular.module('admin-projects', [])

  .controller('ProjectsEditCtrl', function($scope, project) {

    $scope.project = project;
    
    $scope.removeTeamMember = function(teamMember) {

      var idx = $scope.project.teamMembers.indexOf(teamMember);
 
      if(idx >= 0) {
        $scope.project.teamMembers.splice(idx, 1);
      }

    };

    //other methods of the controller

});
```

La lógica del controlador presentado no es compleja y nos vas a permitir enfocarnos en la prueba en sí: 

```
describe('ProjectsEditCtrl tests', function () {

  var $scope;
  
  beforeEach(module('admin-projects'));
  
  beforeEach(inject(function ($rootScope) {
    $scope = $rootScope.$new();
  }));
 
  it('should remove an existing team member', inject(function ($controller) {

    var teamMember = {};

    $controller('ProjectsEditCtrl', {
      $scope: $scope,
      project: {
        teamMembers: [teamMember]
      }
    });

    //verify the initial setup
    expect($scope.project.teamMembers).toEqual([teamMember]);

    //execute and verify results
    $scope.removeTeamMember(teamMember);
    expect($scope.project.teamMembers).toEqual([]);

  }));

});
```

El método `removeTeamMember` que queremos probar aquí se definirá en un `$scope` y `ProjectsEditCtrl` es el controlador que define este método. Para probar la eficacia del método removeTeamMember tenemos que crear un nuevo ámbito, una nueva instancia del controlador `ProjectsEditCtrl` y vincular a los dos, juntos. Básicamente tenemos que hacer manualmente el trabajo de la directiva `ng-controller`.

Por un momento más coloquemos nuestra atención en la sección `beforeEach`, ya que hay cosas interesantes que suceden allí, En primer lugar estamos obteniendo acceso al servicio `$rootScope` y creando una nueva instancia `$scope` `($scope.$new())`. Lo hacemos para simular lo que sucedería en una aplicación en ejecución, donde la directiva `ng-controller` crearía un nuevo ámbito. 

Para crear una instancia de un controlador nosotros podemos usar el servicio `$controller` (por favor nota como la función `inject` puede ser colocada en la sección `beforeEach` así como en el nivel de `it`).

> `NOTA` Mira que fácil es especificar argumentos del constructor del controlador mientras se invoca el servicio `$controller`. Esta es la inyección de dependencia en su mejor momento, podemos proporcionar tanto un ámbito falsos y los datos de prueba para ejercer la implementación del controlador en completo aislamiento.

### Objetos ficticios y pruebas de código asíncronas

Podemos ver cómo AngularJS ofrece una muy buena integración de su sistema de inyección de dependencia con el marco de pruebas de Jasmine. Pero la buena historia de la capacidad de prueba continúa como AngularJS ofrece unos excelentes objetos ficticios también.

La programación asincrónica es el pan y la mantequilla en JavaScript. El código asíncrono desafortunadamente tiende a ser difícil de probar. Los eventos asíncronos no son muy predecibles y pueden ejecutarse en cualquier orden y desencadenar acciones luego de un desconocido período de tiempo. La buena noticia es que el equipo AngularJS provee excelentes objetos ficticios que hacen la pruebas de código asincrónicas muy fáciles y lo que es importante es que son rápidas y predecibles. ¿Como puede ser esto? Vamos a echar un vistazo al ejercicio de prueba de ejemplo, el código usando el servicio $timeout. En primer lugar en el propio código:

```
angular.module('async', [])
  .factory('asyncGreeter', function ($timeout, $log) {
    return {
      say:function (name, timeout) {
        $timeout(function(){
          $log.info("Hello, " + name + "!");
        }, timeout)
      }
    };
  });
```

> `NOTA` El servicio `$timeout` es el reemplazo para la función JavaScript `setTimeOut`. Es preferible usar `$timeout` para el propósito de aplazar acciones ya que está estrechamente integrado con el compilador AngularJS HTML y desencadenará la actualización del DOM luego del que tiempo se acabe. Además de esto, el código resultante es fácil de probar.

He aquí la prueba:

```
describe('Async Greeter test', function () {
  var asyncGreeter, $timeout, $log;

  beforeEach(module('async'));

  beforeEach(inject(function (_asyncGreeter_, _$timeout_, _$log_) {
    asyncGreeter = _asyncGreeter_;
    $timeout = _$timeout_;
    $log = _$log_;
  }));

  it('should greet the async World', function () {
    asyncGreeter.say('World', 9999999999999999999);
    //
    $timeout.flush();
    expect($log.info.logs).toContain(['Hello, World!']);
  });
  
});
```


La mayoría de este código debería ser fácil de seguir pero hay dos interesantes cosas que requieren un poco de atención. Primeramente nosotros podemos ver la solicitud al método `$timeout.flush()`. Esta pequeña solicitud al objeto ficticio `$timeout`, simula un evento asíncrono siendo desencadenado. La parte interesante es que nosotros tenemos control total sobre cuándo este evento va a ser desencadenado. No necesitamos esperar que ocurra el evento timeout, ni estamos a merced de eventos externos. Tenga en cuenta que hemos especificado un tiempo de espera muy largo, sin embargo, nuestra prueba se ejecutará inmediatamente, Ya que no dependemos del método JavaScript `setTimeout`. Más bien es el objeto `$timeout` quien simula el comportamiento del entorno asíncrono.

> `NOTA` Implementar objetos ficticios predecibles es excelente para los servicios asíncronos, son una de las razones porque las pruebas AngularJS se pueden ejecutar increíblemente rápido.

En muchas plataformas hay a menudo servicios fundamentales y globales que son difíciles de probar. El registro y el código para el manejo de excepciones, son ejemplos de esta lógica de prueba que causa dolores de cabeza. Por suerte, AngularJS tiene un remedio, él proporciona servicios para abordar esas preocupaciones de infraestructura junto con el acompañamiento de objetos ficticios. Usted probablemente ha notado que en la prueba de ejemplo hace uso de uno o más objetos ficticios, es decir, `$log`. El objeto ficticio para el servicio `$log` acumulará todas las declaraciones de registro y las almacenará para más aserciones. Usar un objeto ficticio asegura que nosotros no estamos invocando servicios de plataformas reales; especialmente de aquellas que son potencialmentes costosas en términos de desempeño y que podría tener efectos secundarios (por ejemplo, uno podría imaginar que el servicio `$log` envía registros a servidor a través de HTTP, y sería muy mala idea para realizar  llamadas de red, mientras esta realizando pruebas).

### Pruebas de principio a fin 

AngularJS introduce su propia solución de principio a fin para pruebas llamada Corredor de escenarios (Scenario Runner). El Corredor de escenarios puede ejecutar pruebas de integración impulsando acciones en un verdadero navegador. El automáticamente ejecuta acciones (llenando formularios, hacer clic en los botones, en los enlaces y así sucesivamente) y verificar si la interfaz de usuario responde (la página cambia, la información adecuada esta en pantalla, y así sucesivamente), lo que reduce considerablemente la necesidad de las verificaciones manuales que tradicionalmente son realizadas por los equipos de control de calidad.

La solución que ofrece AngularJS tiene similar funcionalidad a otras herramientas existentes (por ejemplo, la muy popular Selenium), pero esta se integra perfectamente con el marco. Más específicamente:

- El Corredor de escenarios es consciente de los eventos asíncronos (lo más importante, las llamadas XHR). El puede pausar la ejecución de una prueba hasta que el evento asíncrono se complete. En la práctica esto significa que el código de prueba no tiene que usar todas las instrucciones de espera explícitas. Cualquiera que haya tratado de probar una aplicación web Ajax-pesada utilizando herramientas tradicionales apreciará altamente esta característica.

- Podemos tomar ventaja de vincular la información ya disponible en las plantillas AngularJS para seleccionar los elementos DOM para manipulaciones adicionales y verificaciones. En esencia, es posible localizar elementos DOM y formularios de entrada basadas en el modelo al que están obligados esos elementos. Esto quiere decir que no necesitamos insertar atributos HTML superfluos (generalmente identificadores o clases CSS) ni confiar en expresiones XPath frágiles, ya que el marco de pruebas puede localizar los elementos DOM.

- La sintaxis utilizada por el corredor de escenarios hace que sea muy fácil encontrar los elementos DOM, interactuar con ellos, y hacer valer en sus propiedades. Hay un completo **lenguaje  específico de dominio** (**DSL**) para buscar y encontrar los repetidores, inputs, etc.

Desafortunadamente, con el pasar del tiempo, más y más limitaciones del corredor de escenarios estaban surgiendo. Debido a esto, al momento de escribir estas líneas, el corredor de escenarios no está activamente mantenido. Hay planes para reemplazarlo con otra solución basada en la integración Selenium, llamada, Protractor, El trabajo en progreso se puede ver en el repositorio GitHub https://github.com/angular/protractor.

> `TIP` No recomendamos el uso del existente corredor de escenarios para cualquier proyecto nuevo. No es activamente mantenido y no va evolucionar. En su lugar, usted debería mantener un ojo en Protractor.

### Flujo de trabajo diario

Con el fin de ser eficaz, las pruebas automatizadas necesitan ser aplicadas rigurosamente. Las pruebas se deben ejecutar con tanta frecuencia como sea prácticamente posible y las pruebas fallidas deberían ser arregladas tan pronto sea posible. Una prueba fallida debe ser tratada como una compilación fallida, y acomodar una compilación rota debe ser una prioridad para el equipo.   

Durante el dia nosotros vamos a cambiar constantemente entre el código JavaScript y las plantillas de la interfaz de usuario. Cuando nos encontremos en el código JavaScript puro y otros artefactos AngularJS (como filtros o directivas), es importante ejecutar a menudo pruebas unitarias. De hecho, debido a la remarcable velocidad en la que AngularJS puede ejecutar pruebas con el corredor de pruebas Karma se vuelve práctico ejecutar todas las pruebas unitarias tan a menudo como en todos los archivos guardados. 

Mientras se escribía la aplicación SCRUM de muestra, los autores de este libro estaban trabajando con el corredor Karma, el cual estaba configurado de una manera que supervisará todos los cambios de los archivos (tanto en el código fuente y el código de prueba). En todos y cada uno de los archivos guardados, un completo conjunto de pruebas fueron ejecutadas  proporcionado una inmediata verificación. En una configuración de este tipo, el bucle de realimentación se hace muy corto, y siempre podremos saber que, cualquiera sea nuestro código aún funciona como se esperaba o se rompió apenas unos segundos antes. Si las cosas están en orden, podemos movernos con rapidez, pero si las pruebas empiezan a fallar sabemos que se debe a uno de los últimos cambios. Con pruebas ejecutándose con tanta frecuencia, se puede decir adiós a las largas y tediosas sesiones de depuración.

Ejecutar las pruebas no debería requerir mucho esfuerzo. Es muy importante tener un confortable entorno de prueba, donde las pruebas automatizadas puedan ser ejecutadas lo más frecuentemente posible. Sí ejecutar una prueba implica varios pasos tediosos manuales, nosotros vamos a evitar la prueba.

Con herramientas modernas es posible tener una configuración extremadamente eficiente. Por ejemplo, aquí está la captura de pantalla del entorno de desarrollo utilizado para escribir los ejemplos de este libro. Como se puede ver, el código fuente y el de prueba se abren al lado del otro, por lo que es fácil pasar de el código de prueba al código de producción. El corredor Karma observa el sistema de archivo para ejecutar pruebas en cada archivo guardado y proveer inmediatamente la respuesta inmediata (en el panel inferior). 
  
![chapter-2-7](/img/chapter-2-7.png)

> `TIP` El entorno de las pruebas unitarias descritas aquí es de hecho el más ligero entorno de desarrollo posible. Nosotros podemos trabajar todo el tiempo en nuestro IDE y enfocarnos en escribir código. No necesitamos cambiar el contexto para construir, implementar, o hacer click en el navegador para verificar que nuestro código funciona como se esperaba. 

### Consejos y trucos de el corredor Karma.

Las prácticas Test Driven Development (TDD) reduce en gran medida el número de largas sesiones de depuración. Cuando escribimos código de prueba, primero nosotros usualmente hacemos pequeños cambios y ejecutamos pruebas, por lo que a menudo el cambio que causa que la prueba falle está a sólo pulsar unas teclas de distancia.  Pero a pesar de nuestros mejores esfuerzos, habrá momentos en los que no vamos a ser capaces de entender lo que está pasando sin tener que recurrir a un depurador. En tiempos como estos, nosotros necesitamos ser capaces de aislar rápidamente una prueba fallida y enfocarnos en esta prueba. 

### Ejecutando un subconjunto de pruebas 

La versión Jasmine que viene con el corredor Karma tiene extensiones muy útiles para aislar rápidamente un conjunto de pruebas para ejecutar: 

- Anteponer a una prueba o suite con un con el carácter `x`, ejemplo: (`xit`,`xdescribe`) desactivará esta prueba/suite durante la próxima ejecución. 

- Anteponer a una suite de pruebas con el carácter `d`, ejemplo: (`ddescribe`) ejecutará solo esta suite. Ignorando a las demás.

- Anteponer una prueba con el carácter `i`, ejemplo: (`iit`) ejecutará sólo esta prueba, Ignorando a las demás pruebas y suites.

Esos pequeños prefijos son extremadamente útiles en la práctica y se pueden utilizar como se muestra a continuación:

```
describe('tips & tricks', function () {

  xdescribe('none of the tests here will execute', function () {

    it("won't execute - spec level", function () {
    });

    xit("won't execute - test level", function () {
    });

  });
 
  describe('suite with one test selected', function () {

    iit('will execute only this test', function () {
    });

    it('will be executed only after removing iit', function () {
    });

  });
 
});
```

### Depuración

Precisar una prueba defectuosa es la mitad del éxito. Todavía tenemos que entender lo que está pasando, y esto a menudo implica una sesión de depuración. Pero cómo depurar las pruebas ejecutadas por corredor Karma? Resulta que es muy fácil, es suficiente agregar la instrucción debugger en nuestra prueba o código de producción.

> `TIP` Mientras usamos la instrucción debugger no se olvide de encender las herramientas de desarrollo en su navegador favorito. De lo contrario, el navegador no se detendrá y usted no será capaz de depurar pruebas.

A veces puede ser más fácil simplemente imprimir valores de algunas variables en una consola. Los objetos ficticios AngularJS vienen con un conveniente método para tales ocasiones `angular.mock.dump(object)`. De hecho, el método dump también está expuesto en el ámbito global (window), así que nosotros simplemente podemos escribir dump(object). 

## Resumen

Este capítulo nos preparó para escribir aplicaciones de la vida real con AngularJS. Al principio nos fuimos sobre los detalles de la aplicación de ejemplo, su dominio del problema y la pila de técnicas. El resto de este libro está lleno de ejemplos inspirados por la aplicación de ejemplo para administrar proyectos usando la metodología SCRUM. Para escribir esta aplicación nosotros vamos a usar una pila de código abierto basada en JavaScript (or al menos amigable), alojada en MongoDB como un almacén de persistencia, Node.js como la solución del lado del servidor y obviamente AngularJS como el marco front-end.

Pasamos una considerable cantidad de tiempo discutiendo filosofía problemática relacionada con la estructura detrás de un sistema de construcción perfecto y sus implementaciones prácticas. Hemos elegido grunt.js, basado JavaScript (y Node.js) para escribir scripts de ejemplo. Mirando la estructura, inevitablemente nos lleva a mirar la organización de los archivos y carpetas en el proyecto de ejemplo. Hemos realizado una estructura de directorios que juega muy bien con el sistema de módulos AngularJS. Después de considerar diferentes maneras de declarar módulos y proveedores AngularJS, hemos resuelto en la sintaxis que evita la confusión, que no contamina el espacio de nombres global, y que mantiene la complejidad de nuestro sistema de construcción a un nivel razonable
 
Los beneficios de las pruebas automatizadas son bien conocidas y entendidas estos días. Somos afortunados desde que AngularJS colocó fuerte hincapié en la capacidad de prueba de el código escrito usando el marco. Vimos que AngularJS viene con una completa oferta de herramientas para pruebas (corredor Karma), e integración con las existentes librerías (Jasmine), y el incrustado sistema de inyección de dependencias que hace fácil probar objetos individuales de forma aislada. Vimos cómo probar artefactos AngularJS en la práctica, así como también probar servicios y controladores AngularJS no debería tener secretos para usted en este momento. Con todas las soluciones en el lugar es fácil colocar todo en orden. 

Hasta ahora hemos aprendido los fundamentos AngularJS y vimos cómo estructurar un aplicación extraordinaria. Nosotros podemos ahora usar estos fundamentos para añadir elementos de la vida real concretos. Ya que cualquiera aplicación CRUD necesita trabajar con la data obtenida a partir de un almacén persistente, el proximo capitulo.   

