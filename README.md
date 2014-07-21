Desarrollo de aplicaciones web con AngularJS
============================================

Traducción al español del libro de Peter Bacon Darwin, Pawel Kozlowski. (August 2013). Mastering Web Application Development with AngularJS. (1ª ed.) UK. Packt Publishing Ltd. ISBN 978-1-78216-182-0


#Tabla de contenido

## Prólogo

## [Capítulo 1: Angular Zen](/chapter-1.md)

- **Conocé AngularJS**
  - Familiarizarse con el marco de trabajo
  - Encuentre su camino en el proyecto
    - La comunidad
    - Recursos de aprendizaje en línea
  - Bibliotecas y extensiones
  - Herramientas
    - Batarang
    - Plunker y jsFiddle
    - Extensiones y plugins IDE
- **Curso intensivo AngularJS**
  - Hola Mundo - el ejemplo AngularJS
    - Enlace de datos de doble vía o bidireccional
  - El patrón MVC en AngularJS
    - Vista panorámica
    - Ámbito
      - Controlador
      - Modelo
      - Ámbitos en profundidad
      - Jerarquía de ámbitos
      - Jerarquía de ámbitos y la herencia
      - Los peligros de la herencia a través de la jerarquía de ámbitos
      - La Jerarquía de los ámbitos y el sistema de eventos
      - Ciclo de vida del ámbito
    - Vista
      - Declarativa vista de plantilla - la lógica del controlador imperativo
  - Módulos e inyección de dependencias
    - Módulos en AngularJS
    - Objetos Colaboradores
      - Inyección de dependencias
      - Beneficios de la inyección de dependencias
    - Registrando servicios
      - Valores
      - Servicios
      - Fábricas
      - Constantes
      - Proveedores
    - Ciclos de vida de los Módulos
      - La fase de configuración
      - La fase de ejecución
      - Diferentes fases y diferentes métodos de registro
    - Módulos que depende de otros módulos
      - Servicios y su visibilidad  a lo largo de los módulos
      - Por que usar módulos AngularJS
- **AngularJs y el resto del mundo**
  - JQuery y AngularJS
    - Manzanas y naranjas
  - Un vistazo al futuro
- **Resumen**


## [Capítulo 2. Construyendo y Probando](/chapter-2.md)

- **Presentamos la aplicación de ejemplo**
  - Familiarizarse con el dominio del problema
  - Conjunto de técnicas
  - Almacén de persistencia
    - MongoLab
    - Entorno del lado del servidor
    - Librerías JavaScript de terceros
    - Bootstrap CSS
- **Sistema de construcción**
  - Construir los principios del sistema
    - Automatizar todo  
    - Falla rápido, Libre de fallos  
    - Diferentes flujos de trabajo, diferentes comandos  
    - Scripts compilados es codigo tambien  
  - Herramientas
    - Grunt.js
    - Librerías y herramientas de prueba
    - Jasmine
    - Corredor Karma      
- **Organización de archivos y carpetas**
  - Carpetas raíz
  - Dentro del la carpeta del código fuente de la aplicación
    - Archivos específicos de AngularJS
    - Empieza simple
      - Los Controladores y parciales evolucionan juntos      
    - Dentro de la carpeta de pruebas
  - Convenciones de los nombres de los archivos
- **Archivos y Módulos AngularJS**
  - Un archivo, un módulo
  - Dentro de un módulo
    - Diferentes sintaxis para registrar proveedores
    - Sintaxis para declarar la configuración y bloques de ejecución
- **Pruebas automatizadas**
  - Pruebas unitarias
    - Anatomía de las pruebas Jasmine
    - Probando objetos AngularJS
    - Probando Servicios
    - Probando controladores
    - Objetos ficticios y pruebas de código asíncronas
  - Pruebas de principio a fin
    - Flujo de trabajo diario
    - Consejos y trucos de el corredor Karma
    - Ejecutando un subconjunto de pruebas
    - Depuración      
- **Resumen**


## Capítulo 3. Comunicación con un servidor back-end

- **Haciendo solicitudes XHR y JSON con $http**
  - Familizializarce com el modelo de datos y la URLs MongoLab
  - Vista rápida a las APIs $http
    - La configuración del objeto
    - Conversión de la data solicitada
    - Tratar con respuestas HTTP
    - La conversión de datos de respuesta        
  - Tratar con las restricciones de la política del mismo origen
    - Superar las restricciones de la política del mismo origen con JSONP  
    - Limitaciones de JSONP  
    - Superar las restricciones de la política del mismo origen con CORS  
    - Proxys del lado del servidor  
- **La API promesa con $q**
  - Trabajando con promesas y el servicio $q
    - Aprendiendo lo básico del servicio $q  
    - Las promesas (Promises) son objetos JavaScript de primera clase  
    - Agregando retrollamadas  
    - Ciclo de vida de las promesas y retrollamadas registradas  
    - Encadenando acciones asincronas  
    - Más sobre $q
      - Agregando promesas
      - Envasar valores como promesas      
  - Integración del servicio $q en AngularJS
- **La API promise con $http**
- **Comunicación con los puntos finales del servicio REST**
  - El servicio $resource
    - Métodos a nivel de la instancia y a nivel del constructor
      - Métodos a nivel del constructor
      - Métodos a nivel de la instancia
      - Métodos personalizado
      - Añadiendo comportamiento a los objetos de recursos
    - $resource crea métodos asíncronos
    - Limitaciones del servicio $resource
  - Adaptadores REST personalizados con $http
- **Usando características avanzadas de $http**
  - Interceptando respuestas
- **Probando el código que interactúa con $http**
- **Resumen**

## Capítulo 4. Mostrar y dar formato a datos

- **Hacer referencia a las directivas**
- **Desplegando resultados de expresiones de evaluación**
  - La directiva de interpolación
  - Representando valores del modelo con ngBing
  - Contenido HTML en expresiones AngularJS
- **Visualización condicional**
  - Incluyendo bloques de contenido condicionalmente
- **Representando colecciones con la directiva ngRepeat**
  - Familiarizarse con la directiva ngRepeat
  - Variables especiales
  - Iteración sobre propiedades de un objeto
  - Patrones ngRepeat
    - Las listas y detalles
      - Desplegando solo una fila con detalles
      - Desplegando muchas filas con detalles
    - Alterando tablas, columnas, y clases
- **Controladores de eventos DOM**
- **Trabajando efectivamente con plantillas basadas en DOM**
  - Vivir con una sintaxis detallada
  - ngRepeat y múltiples elementos DOM
  - Elemento y atributos que no pueden ser modificados en tiempo de ejecución
  - Elementos personalizados HTML y versiones antiguas de internet explorer
- **Manejando las transformaciones del modelo con filtros**
  - Trabajando con filtros incorporados
    - Filtros de formato 
    - Filtros de formato 
      - Filtrado con el filtro “filter”    
      - Contando resultados filtrados    
      - Ordenando con el filtro orderBy    
  - Escribiendo filtros personalizados - un ejemplo de paginación
  - Acceso a filtros desde código JavaScript
  - Filtros, Hacer y qué evitar
    - Filtros y manipulación DOM
    - Costosa transformación de datos en filtros
    - Filtros inestables
- **Resumen**


## Capítulo 5. Creando formularios avanzados

- **Comparando formularios tradicionales con formularios AngularJS**
  - Presentado la directiva ngModel
- **Creación un Formulario de Información de usuario**
- **Comprendiendo las directivas de campo**
  - Añadiendo la validación requerida
  - Usando campos basados en texto (text, textarea, e-mail,URL, number)
  - Usando campos checkbox
  - Usando campos radio
  - Usando campos select
    - Proporcionando simples opciones de cadena  
    - Proporcionando opciones dinámicas con la directiva ngOptions  
    - Ejemplos comunes de ngOptions  
    - Entendiendo la expresiones de dataSource  
    - Entendiendo la expresión optionBinding  
    - Usando opciones vacías con la directiva select  
    - Entendiendo select y el objeto de equivalencia  
    - Seleccionando múltiples opciones  
  - Trabajando con campos de entrada (inputs) ocultos HTML tradicionales
    - Incorporando valores desde el servidor
    - Enviando un formulario HTML tradicional      
- **Mirando dentro del enlace de datos de ngModel**
  - Entendiendo ngModelController
    - Transformando el valor entre el modelo y la vista
    - Seguimiento de si el valor ha cambiado
    - Seguimiento a la validez de un campo de entrada
- **Validando formularios AngularJS**
  - Entendiendo el controlador ngFormController
    - Usando el atributo name para vincular formularios al ámbito
  - Añadiendo comportamiento dinámico al formulario de información de usuario
    - Mostrando errores de validación
    - Deshabilitando el botón de guardado
  - Deshabilitando la validación nativa del navegador
- **Formularios Anidados en otros Formularios**
  - Usando Sub-Formularios como componentes reusables
- **Repitiendo subformularios**
  - Validando inputs repetidos
- **Manejar el envío de formularios HTML tradicionales**
  - Enviando formularios directamente al el servidor
  - Manejar eventos de envio de formulario
      - Usando ngSubmit para manejar el envío del formulario
      - Usando ngClick para manejar el envío del formulario
- **Restableciendo el formulario de información del usuario**
- **Resumen**


## Capítulo 6. Navegación Organizada

- **URLs en aplicaciones web de una sola-página**
  - Hashbang URLs en la era pre-HTML5
  - HTML5 y la API Historia
- **Usando el servicio $location**
  - Entendiendo la URLs y API del servicio $location
  - Los valores hash, navegación dentro de una página, Y $anchorScroll
  - Configurando el modo HTML5 para las URLs
    - Lado del cliente
    - Lado del servidor
  - Navegación hecha a mano usando el servicio $location
    - Estructurar páginas alrededor de las rutas
    - Asignando rutas a URLs
    - Definiendo controladores en parciales de ruta
    - Los bits que faltan en la navegación hecha a mano
- **Usando los servicios de enrutado AngularJS integrados**
  - Definición de rutas básicas
    - Desplegando el contenido de la ruta que concuerda
  - Coincidencia de rutas flexibles
    - Definiendo rutas por defecto
    - Acessando a valores de parámetros de la ruta
  - Rehusando parciales con diferentes controladores
  - Evitar el parpadeo de la interfaz de usuario en los cambios de ruta
  - Evitar los cambios de ruta
- **Limitaciones del servicio $route**
  - Una ruta corresponde a un rectángulo en la pantalla
    - Administrando múltiples rectángulos de la interfaz de usuario con ng-include
  - No hay soporte para rutas anidadas
- **Patrones específicos de enrutamiento, consejos, y trucos**
  - Manejo de links
    - Creación de enlaces en los que puede hacer clic
    - Trabajando conscientemente con los modos HTML5 Y hashbang
    - Enlazando páginas externas
  - Organizando definiciones de ruta
    - Esparciendo la definiciones de ruta entre varios módulos  
    - Luchando con la duplicaciones de código en la definiciones de ruta  
- **Resumen**


## Capítulo 7. Asegurar su aplicación

- **Proporcionado autenticacion y autorizacion del lado del servidor**
  - Manejando accesos no autorizados
  - Proporcionado una API de autenticación del lado del servidor
- **Asegurando plantilla parciales**
- **Detener ataques maliciosos**
  - Prevenir el espionaje de cookie (ataques de hombre-en-el-medio)
  - Previniendo ataques de secuencia de comandos entre sitios
    - Asegurar el contenido HTML en expresiones AngularJS
    - Permitir enlaces HTML inseguros
    - Desinfección del HTML
  - Evitar la vulnerabilidad de inyección de JSON
  - Evitar solicitudes falsificadas entre sitios
- **Añadiendo soporte de seguridad al lado del cliente**
  - Creando un servicio de seguridad
  - Mostrando el formulario de inicio de sesión
  - Creando menús y barras de herramientas conscientes sobre la seguridad
    - Escondiendo elementos del menú
    - Creando una barra de herramientas de sesión
- **Soportando autenticación y autorización en el cliente**
  - Manejando autorizaciones fallidas
  - Interceptando respuestas
    - Interceptores de respuesta HTTP
  - Creando un servicio securityInterceptor
  - Creando el servicio securityRetryQueue
    - Notificar al servicio de seguridad
- **Evitar la navegación a rutas seguras**
  - Usar funciones resolve de ruta
  - Crear el servicio de autorización
- **Resumen**


## Capítulo 8 - Construya sus propias directivas

- **¿Qué son las directivas AngularJS?**
  - Comprendiendo las directivas integradas
  - Usar directivas en el marcado HTML
- **Siguiendo el ciclo de vida de compilación de la directiva**
- **Escribir pruebas unitarias para las directivas**
- **Definir una directiva**
- **Estilizar botones con directivas**
  - Escribiendo una directiva botón
- **Entendiendo directivas widget AngularJS**
  - Escribiendo una directiva de paginación
  - Escribir pruebas para la directiva de paginación
  - Usar una plantilla HTML en una directiva
  - Aislar nuestra directiva de su ámbito padre
    - Interpolando el atributo con @
    - Enlazando datos al el atributo con =
    - Proporcionando una expresión de retrollamada en el atributo con &
  - Implementar el widget
  - Añadir una retrollamada selectPage a la directiva
- **Creando una directiva de validación personalizada**
  - Requiriendo una directiva controladora
    - Haciendo el controlador opcional
    - Buscando ancestros o padres para el controlador
  - Trabajando con ngModelController
  - Escribiendo pruebas de directivas de validación personalizada
  - Implementando una directiva de validación personalizada
- **Crear un validador de modelo asíncrono**
  - Simular el servicio User
  - Escribir pruebas para validación asíncrona
  - Implementado la directiva de validación asíncrona
- **Envolviendo la directiva datepicker de jQueryUI**
  - Escribir pruebas para directivas que envuelven librerías
  - Implementando la directiva datepicker jQuery
- **Resumen**