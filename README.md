Desarrollo de aplicaciones web con AngularJS
============================================

Traducción al español del libro de Peter Bacon Darwin, Pawel Kozlowski. (August 2013). Mastering Web Application Development with AngularJS. (1ª ed.) UK. Packt Publishing Ltd. ISBN 978-1-78216-182-0


#Tabla de contenido

##[**Prólogo**]()

##[Capítulo 1: Angular Zen]()

- [**Conocé AngularJS**]()
  - [Familiarizarse con el marco de trabajo]()
  - [Encuentre su camino en el proyecto]()
    - [La comunidad]()
    - [Recursos de aprendizaje en línea]()
  - [Bibliotecas y extensiones]()
  - [Herramientas]()
    - [Batarang]()
    - [Plunker y jsFiddle]()
    - [Extensiones y plugins IDE]()
- [**Curso intensivo AngularJS**]()
  - [Hola Mundo - el ejemplo AngularJS]()
    - [Enlace de datos de doble vía o bidireccional]()
  - [El patrón MVC en AngularJS]()
    - [Vista panorámica]()
    - [Ámbito]()
      - [Controlador]()
      - [Modelo]()
      - [Ámbitos en profundidad]()
      - [Jerarquía de ámbitos]()
      - [Jerarquía de ámbitos y la herencia]()
      - [Los peligros de la herencia a través de la jerarquía de ámbitos]()
      - [La Jerarquía de los ámbitos y el sistema de eventos]()
      - [Ciclo de vida del ámbito]()
    - [Vista]()
      - [Declarativa vista de plantilla - la lógica del controlador imperativo]()
  - [Módulos e inyección de dependencias]()
    - [Módulos en AngularJS]()
    - [Objetos Colaboradores]()
      - [Inyección de dependencias]()
      - [Beneficios de la inyección de dependencias]()
    - [Registrando servicios]()
      - [Valores]()
      - [Servicios]()
      - [Fábricas]()
      - [Constantes]()
      - [Proveedores]()
    - [Ciclos de vida de los Módulos]()
      - [La fase de configuración]()
      - [La fase de ejecución]()
      - [Diferentes fases y diferentes métodos de registro]()
    - [Módulos que depende de otros módulos]()
      - [Servicios y su visibilidad  a lo largo de los módulos]()
      - [Por que usar módulos AngularJS]()
- [**AngularJs y el resto del mundo**]()
  - [JQuery y AngularJS]()
    - [Manzanas y naranjas]()
  - [Un vistazo al futuro]()
- [**Resumen**]()


##[Capítulo 2.  Construyendo y Probando]()

- [**Presentamos la aplicación de ejemplo**]()
  - [Familiarizarse con el dominio del problema]()
  - [Conjunto de técnicas]()
  - [Almacén de persistencia]()
    - [MongoLab]()
    - [Entorno del lado del servidor]()
    - [Librerías JavaScript de terceros]()
    - [Bootstrap CSS]()
- [**Sistema de construcción**]()
  - [Construir los principios del sistema]()
    - [Automatizar todo]()  
    - [Falla rápido, Libre de fallos]()  
    - [Diferentes flujos de trabajo, diferentes comandos]()  
    - [Scripts compilados es codigo tambien]()  
  - [Herramientas]()
    - [Grunt.js]()
    - [Librerías y herramientas de prueba]()
    - [Jasmine]()
    - [Corredor Karma]()      
- [**Organización de archivos y carpetas**]()
  - [Carpetas raíz]()
  - [Dentro del la carpeta del código fuente de la aplicación]()
    - [Archivos específicos de AngularJS]()
    - [Empieza simple]()
      - [Los Controladores y parciales evolucionan juntos]()      
    - [Dentro de la carpeta de pruebas]()
  - [Convenciones de los nombres de los archivos]()
- [**Archivos y Módulos AngularJS**]()
  - [Un archivo, un módulo]()
  - [Dentro de un módulo]()
    - [Diferentes sintaxis para registrar proveedores]()
    - [Sintaxis para declarar la configuración y bloques de ejecución]()
- [**Pruebas automatizadas**]()
  - [Pruebas unitarias]()
    - [Anatomía de las pruebas Jasmine]()
    - [Probando objetos AngularJS]()
    - [Probando Servicios]()
    - [Probando controladores]()
    - [Objetos ficticios y pruebas de código asíncronas]()
  - [Pruebas de principio a fin]()
    - [Flujo de trabajo diario]()
    - [Consejos y trucos de el corredor Karma]()
    - [Ejecutando un subconjunto de pruebas]()
    - [Depuración]()      
- [**Resumen**]()


##[Capítulo 3. Comunicación con un servidor back-end]()

- [**Haciendo solicitudes XHR y JSON con $http**]()
  - [Familizializarce com el modelo de datos y la URLs MongoLab]()
  - [Vista rápida a las APIs $http]()
    - [La configuración del objeto]()
    - [Conversión de la data solicitada]()
    - [Tratar con respuestas HTTP]()
    - [La conversión de datos de respuesta]()        
  - [Tratar con las restricciones de la política del mismo origen]()
    - [Superar las restricciones de la política del mismo origen con JSONP]()  
    - [Limitaciones de JSONP]()  
    - [Superar las restricciones de la política del mismo origen con CORS]()  
    - [Proxys del lado del servidor]()  
- [**La API promesa con $q**]()
  - [Trabajando con promesas y el servicio $q]()
    - [Aprendiendo lo básico del servicio $q]()  
    - [Las promesas (Promises) son objetos JavaScript de primera clase]()  
    - [Agregando retrollamadas]()  
    - [Ciclo de vida de las promesas y retrollamadas registradas]()  
    - [Encadenando acciones asincronas]()  
    - [Más sobre $q]()
      - [Agregando promesas]()
      - [Envasar valores como promesas]()      
  - [Integración del servicio $q en AngularJS]()
- [**La API promise con $http**]()
- [**Comunicación con los puntos finales del servicio REST**]()
  - [El servicio $resource]()
    - [Métodos a nivel de la instancia y a nivel del constructor]()
      - [Métodos a nivel del constructor]()
      - [Métodos a nivel de la instancia]()
      - [Métodos personalizado]()
      - [Añadiendo comportamiento a los objetos de recursos]()
    - [$resource crea métodos asíncronos]()
    - [Limitaciones del servicio $resource]()
  - [Adaptadores REST personalizados con $http]()
- [**Usando características avanzadas de $http**]()
  - [Interceptando respuestas]()
- [**Probando el código que interactúa con $http**]()
- [**Resumen**]()