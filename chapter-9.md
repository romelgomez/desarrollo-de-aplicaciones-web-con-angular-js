
Capítulo 9 - Construir Directivas Avanzadas 

El capítulo previo introduce como desarrollar y probar sus propias directivas personalizadas. En este capítulo nos fijamos en algunas de las cosas más avanzadas que usted puede hacer cuando desarrolle directivas AngularJS. Esto incluirá: 

- Entender la transclusión: en particular el uso de las funciones de transclusión y los ámbitos de transclusión.
- Definir sus propias directivas controladoras para crear directivas que puedan cooperar  y cómo estos controladores difieren de las funciones enlazadas. 
- Terminar el proceso de compilación y tomar el control de: cargar sus propias plantillas dinámicamente y usar los servicios $compile y $interpolate. 
Usando transclusion

Cuando mueve elementos desde una parte del DOM a otro, debes decidir que sucede a su ámbito asociado. 

El enfoque navitivo es asociar elementos con el ámbito que es definido en la nueva posición. Esto es parecido a romper la aplicación, ya que los elementos pueden no tener acceso a los elementos desde su ámbito original. 

Lo que realmente necesitamos es "traer el ámbito original con nosotros". Mover los elementos y sus ámbitos de esta forma, es lo que se llama transclusión. Vamos a examinar cómo mover el ámbito en transclusion luego en este capítulo en la sección Entendiendo el ámbito de transclusion. Pero primero vamos a ver algunos ejemplos.
Usar transclusión en directivas

La transclusión es necesaria cada vez que una directiva está reemplazando su contenido original con nuevos elementos, y queramos usar el contenido original en alguna parte de los nuevos elementos. 

Por ejemplo, la directiva ng-repeat transcluirá y clonara su elemento original, estampando múltiples copias del elemento transcluido como este itera sobre una lista de artículos. Cada uno de estos elementos será asociado con un nuevo ámbito, el cual es hijo del ámbito del elemento original. 


Transclución en una directiva de ámbito aislado

La directiva ng-repeat es algo inusual, en que ella hace clones de sí misma, que luego se transcluyen. Es más común usar transclusión cuando estás creando una directiva de widget de plantilla, donde usted quiera insertar el contenido del elemento original en algún punto en la plantilla.  
Crear una directiva de alerta que usa transclusión

Un simple ejemplo de tal widget de plantilla es una directiva de alerta. 

NOTA: Los mensajes de alerta son, aquellos que son desplegados al usuario para indicar el actual estado de la aplicación. 


El contenido de elemento alert contiene el mensaje a desplegar en la alerta. Este necesita ser incluido en la plantilla de la directiva. Una lista de alertas puede ser desplegada usando la directiva ng-repeat:

<alert type="alert.type" close="closeAlert($index)" ng-repeat="alert in alerts">
{{alert.msg}}
</alert>

El atributo close debe contener una expresión que será ejecutada cuando el usuario cierra la alerta. La implementación de esta directiva es algo simple de seguir:

myModule.directive('alert', function () {
    return {
        restrict:'E',
        replace: true,
        transclude: true,
        template:
            '<div class="alert alert-{{type}}">' +
            '<button type="button" class="close"' +
            'ng-click="close()">&times;' +
            '</button>' +
            '<div ng-transclude></div>' +
            '</div>',
        scope: { type:'=', close:'&' }
    };
});

Entender la propiedad replace en la definición de la directiva

La propiedad replace dice al compilador que reemplace el elemento de la directiva original con la plantilla dada por el campo template. Si proporcionamos template pero no replace, entonces el compilador anexara la plantilla en el elemento de la directiva. 

NOTA: Cuando preguntes al compilador reemplazar el elemento con una plantilla, este copiará todos los atributos del elemento original al elemento de la plantilla también.

Entender la propiedad transclude en la definición de la directiva

La propiedad transclude toma cualquiera: true o 'element'. Esto dice al compilador que extraiga el contenido del elemento <alert> original y lo haga disponible para ser transcluido en la plantilla.

- Usando (transclude: true) significa que los elementos hijos de la directiva serań transcluidos. Esto es lo que sucede en la directiva alert, aun reemplazando los elementos de la directiva con nuestra plantilla. 

- Usando (transclude: 'element') significa que todo el elemento será transcluido incluyendo cualquier atributo de la directiva que no ha sido compilado. Esto es lo que sucede en la directiva ng-repeat.
Insertando elementos transcluidos con ng-transclude

La directiva ng-transclude obtiene los elementos transcluidos y los anexa al elemento en la plantilla en la cual éste aparece. Este es la forma más común y simple de usar la transclución. 
Entender el ámbito de la transclusion 

Todos los elementos DOM que han sido compilados por AngularJS tiene un ámbito asociado con ellos. En la mayoría de los casos los elementos DOM no tiene ámbitos definidos directamente a ellos, pero obtienen sus ambito de algún elemento padre. Los nuevos ámbitos son creados por directivas que especifican una propiedad scope en su objeto de definición de directiva.

NOTA: Solo algunas directivas del núcleo definen nuevos ambitos, estas son ng-controller, ng-repeat, ng-include, ng-view, ng-switch. Todos ellos crean ámbitos hijos que prototipicamente heredan de sus ámbitos padres. 

Vimos en el Capítulo 8, Construir tus propias directivas, como construir directivas widget que usan un ámbito aislado para asegurar que los ámbitos dentro y fuera del widget no se contaminen unos a otros. Estos significa que la expresión dentro de la plantilla no tiene acceso a los valores en el ámbito padre, conteniendo el widget. Esto es útil porque no queremos propiedades en el ámbito padre afecten o comience a ser afectado por lo que hacemos dentro de la plantilla.   

NOTA: El contenido original de la directiva, el cual será insertado en la plantilla, necesita ser asociado con el ámbito original y no con el ámbito aislado. Al transcluir los elementos originales podremos mantener el ámbito correcto para estos elementos. 

Nuestra directiva de alerta es un widget, que usa un ámbito aislado. Considera que  ámbitos son creados con la directiva de alerta. Antes que la directiva de alerta sea complida el DOM y su ambito lucen tal cual: 

<!-- defines $rootScope -->
<div ng-app ng-init="type='success'">
    <!-- bound to $rootScope -->
    <div>{{type}}</div>
    <!-- bound to $rootScope -->
    <alert type="'info'" ...>Look at {{type}}</alert>
</div>

El elemento <div>{{type}}</div> no tiene un ámbito directamente definido en el. En su lugar, esta es implícitamente unido al $rootScope porque es un hijo del elemento np-app donde $rootScope es definido y por lo que {{type}} será evaluado a 'success'.
 
En el elemento de alerta tenemos un atributo type="'info'". Este atributo es mapeado a una propiedad type en el ámbito del la plantilla. Una vez que la directiva alert ha sido compilada, esta es reemplazada con su plantilla, el DOM y su ámbito lucen tal cual:

<!-- defines $rootScope -->
<div ng-app ng-init="type='success'">
    <!-- bound to $rootScope -->
    <div>{{type}}</div>
    <!-- defines an isolated scope -->
    <div class="alert-{{type}}" ...>
        <!-- bound to isolated scope -->
        <button>...</button>
        <div ng-transclude>
            <!-- defines new translude scope -->
            <span>Look at {{type}}</span>
        </div>
    </div>
</div>

Dentro del la plantilla, el atributo class="alert-{{type}}" está implícitamente unido a el ámbito aislado y por lo que evaluará a class="alert-info".

En contraste, el contenido transcluido del elemento original  <alert>, <span>Look at {{type}}</span>, ahora está unido a un nuevo ámbito transcluido. Si tenemos que mover nativamente este contenido dentro de la plantilla, su ámbito implícito puede cambiar de $rootScope a el ámbito aislado y entonces {{type}} será evaluado a 'info'. Esto no es lo que queremos. 

En su lugar, el nuevo ámbito transcluido es un hijo de $rootScope y prototípicamente hereda de el. Esto significa que el spam será evaluado correctamente aspan>Look at success </span>. La siguiente imagen muestra estos ámbitos en Batarang:



En la imagen previa, Scope (002) es el $rootScope, el cual contiene el type="success". El ámbito (003) es el ámbito aislado para la plantilla alert y no hereda de $rootScope. El ámbito (004) es el ámbito transcluido y no hereda de $rootScope y por lo tanto este ámbito, type será success.

NOTA: Cuando los elementos son movidos por transclución llegan a llevar su ámbito original con ellos. Más exactamente, los elementos transcluidos serán unidos a un nuevo ámbito que es un hijo prototípico del ámbito de donde fueron tomados. 
Crear y trabajar con funciones de transclusión

La transclución en AngularJS es hecha posible por el uso de funciones de transclusión. Estas funciones son simples funciones enlazadas, la cuales son creadas llamado el servicio $compile. 

Cuando una directiva solicita una transclución, AngularJS extraerá los elementos transcluidos del DOM y los compilará. Esto es una aproximación de que sucede con transclude : true:



var elementsToTransclude = directiveElement.contents();
directiveElement.html('');
var transcludeFunction = $compile(elementsToTransclude);


La primera línea obtiene el contenido del elemento de la directiva que solicita la transclusion. La segunda línea limpia el elemento. La tercera línea compila el contenido transcluido para producir la función de transclusion, la cual será pasada a la directiva, para que esta la use. 

Creando una función con el servicio $compile. 

El compilador AngularJS se expone como el servicio $compile. Este es la misma función que es usada cuando se compila cualquier otra parte en una aplicación AngularJS. Para usar este servicio nosotros simplemente lo llamamos con una lista de nodos DOM (o una cadena que será analizada en una lista de nodos DOM).

var linkingFn = $compile('<div some-directive>Some {{"interpola-ted"}} values</div>');

La llamada al servicio $compile retorna una función enlazada. Puedes llamar esta función con un ámbito para recuperar un elemento DOM que contienen elementos DOM compilados, unidos a un ámbito dado:

var compiledElement = linkingFn(someScope);

NOTA: Las funciones de transclución son sólo instancias especiales de funciones enlazadas. 
Clonar los elementos originales cuando transcluyen

Si pasamos una función retrollamada como un parámetro a una función enlazada, entonces un clon de los elementos serán retornados en lugar del los elementos originales. La  retrollamada será llamada sincrónicamente con los elementos clonados como parámetro.

var clone = linkingFn(scope, function callback(clone) {
element.append(clone);
});


TIP: esto es muy útil si quieres hacer copias de los elementos originales hijos, como debería suceder en ng-repeat.
Acceder a funciones de transclución en directivas

El compilador pasa esta función de transclución de nuevo a la directiva. Hay dos lugares donde usted puede mantener una función de transclución: la función de compilación y la directiva controladora. Hay una sección que describe las directivas controladoras en detalle luego en este capítulo. 

myModule.directive('myDirective', function() {
    return {
        transclude: true,
        compile: function(element, attrs, transcludeFn) { ... };
    controller: function($scope, $transclude) { ... },
    };
});

Aquí nosotros indicamos que la directiva debería transcluir su contenido. Podemos accesar la funciones de transclución en la función de compilación, vía el parámetro transcludeFn y en la directiva controladora, vía el parámetro $transclude. 
Obtener la función de transclución en la función de compilación con transcludeFn

La función de transclusion está disponible como el tercer parámetro de la función de compilación de una directiva. En esta etapa de la compilación, el ámbito no es conocido por lo que la función de transclución no está unida a ningún ámbito. En su lugar, pasará el ámbito de esta función, como su primer parámetro, cuando la llames. 

El ámbito está disponible en la función enlazada y por lo que este es el lugar donde usted generalmente encuentra la función de transclución empezando a ser invocada. 

compile: function(element, attrs, transcludeFn) {
    return function postLink(scope, element, attrs, controller) {
        var newScope = scope.$parent.$new();
        element.find('p').first().append(transcludeFn(newScope));
    };
}




Anexamos los elementos transcluidos al primer elemento <p> bajo la directiva. Cuando llamamos la función de transclución, unimos los elementos transcluidos a un ámbito. En esta caso creamos un nuevo ámbito, el cual es un hermano del ámbito del la directiva, es decir, hija de $parent del ámbito de la directiva.   

Esto es necesario cuando la directiva tiene un ámbito aislado; ya que el ámbito pasado a la función enlazada es un ámbito aislado y no hereda las propiedades del ambito padre, que los elementos transcluidos necesitan.   

Obtener la función de transclución en el controlador de la directiva con $transclude. 
 
Podemos acceder la función de transclución en el controlador de la directiva inyectando $transclude. En esta caso, $transclude es una función que es pre unida a un nuevo hijo del ámbito padre, por lo que no necesitas proporcionar un ámbito. 

controller: function($scope, $element, $transclude) {
$element.find('p').first().append($transclude());
}


Nuevamente, anexamos los elementos transcluidos a el primer elemento <p>.

NOTA: Con $transclude, el ámbito pre unido será un hijo prototípico del ámbito original de donde los elementos transcluidos vienen.   

Crear una if directiva que usa transclución

Vamos a ver una simple directiva que hace uso explícito de las funciones de transclución en lugar de depender en la directiva ng-transclude. Mientras AngularJS 1.0 proporciona ambas directivas ng-show y ng-switch para cambiar la visibilidad del contenido en una aplicación, ng-show no remueve el elemento del DOM cuando lo oculta y ng-switch es algo redundante para simple situaciones. 

Si queremos solo remover el elemento del DOM cuando no se necesite, podemos crear una directiva if. Sería utilizada similarmente a ng-show :


<body ng-init="model= {show: true, count: 0}">
<button ng-click="model.show = !model.show">
Toggle Div
</button>
<div if="model.show" ng-init="model.count=model.count+1">
Shown {{model.count}} times
</div>
</body>

Aquí, cada vez que el botón es cliqueado, el valor de model.show alterna entre true o false. Para mostrar que el elemento DOM es removido y reinsertado en cada click, incrementamos model.count. 

En la prueba unitaria necesitamos probar que el elemento DOM es añadido y removido correctamente: 

it('creates or removes the element as the if condition changes',
function () {
element = $compile(
'<div><div if="someVar"></div></div>')(scope);
scope.$apply('someVar = true');
expect(element.children().length).toBe(1);
scope.$apply('someVar = false');
expect(element.children().length).toBe(0);
scope.$apply('someVar = true');
expect(element.children().length).toBe(1);
});

Aquí revisamos que el número de hijos de nuestro contenedor de elementos incrementa o disminuye como la expresión alterna entre true y false. 

TIP: nota que necesitamos envolver el elemento que contiene la directiva if en un div porque nuestra directiva usará jqLite.after() para insertarla dentro del DOM, la cual requiere que el elemento tenga un padre.    

Veamos cómo implementar esta directiva: 

myModule.directive('if', function () {
return {
transclude: 'element',
priority: 500,
compile: function (element, attr, transclude) {
return function postLink(scope, element, attr) {
var childElement, childScope;
scope.$watch(attr['if'], function (newValue) {
if (childElement) {

childElement.remove();
childScope.$destroy();
childElement = undefined;
childScope = undefined;
}
if (newValue) {
childScope = scope.$new();
childElement = transclude(childScope, function(clone){
element.after(clone);
});
}
});


La directiva transcluye todo el elemento ( transclude: 'element' ). Proporcionamos una función de compilación, la cual nos da acceso a la función de transclución, la cual retorna la función enlazada, donde observamos ($watch) la expresión de atributo if. 

TIP: Usamos $watch en lugar de $observe aquí porque el atributo if debe contener una expresión para ser evaluada en lugar de una cadena para ser interpolada. 

Cuando la expresión cambia, organizamos el ámbito y el elemento hijo, si estos existen. Es  importante asegurarse que no tengamos no tenemos ninguna pérdida de memoria. Si la expresión evalúa a true, podemos crear un nuevo ámbito hijo y entonces usarlo con la función de transclución para clonar una nueva copia de elementos transcluidos. Insertamos estos elementos luego de los elementos que contienen la directiva. 

Usar la propiedada de prioridad en una directiva. 

Todas la directivas tiene una prioridad, por defecto a cero, como es el caso de la directiva alert. En cada elemento, AngularJS compila las directivas de prioridad alta antes de las de prioridad baja. Podemos especificar esto usando la propiedad priority en el objeto de definición de la directiva. 

 Si la directiva tiene: transclude: 'element', el compilador sólo transcluíra los atributos de las directivas que tiene baja prioridad que la actual directiva, en otras palabras, los elementos de las directivas no han sido todavía procesadas.   
   
La directiva ng-repeat tiene: transclude: 'element' y priority: 1000, por lo que generalmente todos los atributos que aparecen en el elemento ng-repeat son transcluidos para aparecer en los elementos repetidos clonados. 

NOTA: Hemos dado a la directiva if una prioridad de 500, la cual es menos que la de ng-repeat. Esto significa que si pones en el mismo elemento como en ng-repeat, la expresión  que if observa referirá el ámbito creado por cada iteración de ng-repeat. 

En esta directiva, la transclución nos permite mantener el contenido de la directiva, unida al ámbito correcto, y condicionalmente insertandolo en el DOM. 

Próximamente vamos cambiar el rumbo y ver los controladores proporcionados específicamente para las directivas.  

Entendiendo los controladores de las directivas. 

Un controlador en AngularJS es un objeto ligado a un elemento DOM que inicializa y añade comportamiento a el ámbito de tal elemento. 

NOTA: Ya hemos visto muchos controladores de aplicación, instanciado por la directiva ng-controller. Estos controladores no deben interactuar directamente con el DOM, solo deben tratar con el actual ámbito. 

Un controlador de directiva es una especial forma de controlador que es definida por una directiva y instanciada cada vez que la directiva aparece en un elemento DOM. Su rol es de inicializar y proporcionar comportamiento a la directiva en lugar de un ámbito.

Definas un controlador de directiva usando la propiedad controller en el objeto de definición de directiva. La propiedad controller puede ser una cadena que contenga el nombre de un controlador ya definido en un módulo: 

myModule.directive('myDirective', function() {
return {
controller: 'MyDirectiveController'
};
});
myModule.controller('MyDirectiveController', function($scope) {
...
});

O puede ser una función constructora que será usada para instanciar el controlador:   

myModule.directive('myDirective', function() {
return {
controller: function($scope, ...) { ... }
};
});

TIP: Si el controlador es definido en un módulo entonces es fácil de probar independientemente de la directiva. Pero esto también significa que el controlador está expuesto a toda la aplicación, a través del inyector, por lo que debes ser cuidadoso  que su nombre no entre en conflicto con controladores en otros modules de la aplicación. 

 Definir el módulo en línea, como una función anónima, hace más difícil hacerle pruebas separadas de la directiva pero nos permite mantenerlo privado a la directiva.   


Inyectar dependencias especiales dentro controladores de directivas.

Tal como cualquier controlador, los controladores de directivas son inyectado con dependencias por AngularJS. Todos los controladores son inyectados con $scope y puedes especificar otros servicios para ser inyectados, tales como $timeout o $rootScope. Además de estos, los controladores de directivas también puede ser inyectado con los siguiente tres servicios especiales: 

$element: Esta es una referencia al elemento DOM de la directiva. Esto será envuelto en jQLite/jQuery. 

$attrs: Esta es una lista normalizada de los atributos que aparecen en elemento DOM de la directiva. 

$transclude: Esto es una función de transclución que está ya unida a el ámbito correcto. Esta función está describida en la funciones de transclución.  
 

Crear una directiva de paginación basada en el controlador 

Hay grandes coincidencias en la funcionalidad entre el controlador (controller:) y la función enlazada (link:) de la directiva. A menudo es posible usar el controlador en lugar de la función enlazada. Aquí está la directiva de paginación del Capítulo 9, Construyendo Directivas Avanzadas, pero esta vez usando un controlador de directiva en lugar de una función enlazada. 

myModule.directive('pagination', function() {
return {
restrict: 'E',
scope: { numPages: '=', currentPage: '=', onSelectPage: '&' },
templateUrl: 'template/pagination.html',
replace: true,
controller: ['$scope, '$element', '$attrs',
function($scope, $element, $attrs) {
$scope.$watch('numPages', function(value) {
$scope.pages = [];
for(var i=1;i<=value;i++) {
$scope.pages.push(i);
}
if ( $scope.currentPage > value ) {
$scope.selectPage(value);
}
});
$scope.noPrevious = function() {
return $scope.currentPage === 1;
};
...
}]
...
});

En este simple caso, la única diferencia entre esta versión y la otra 

la única diferencia entre esta versión y la que utiliza una función enlazada es que, mientras que la función enlazada pasa los parámetros scope, element, attrs, y controller, el controlador de la directiva debe usar anotaciones de inyección de dependencias para ser proporcionado con los servicios $scope, $element, y $attrs.

Entendiendo la diferencia entre los controladores de directiva y funciones enlazadas. 

Al elegir entre usar entre funciones enlazadas o controladores de directiva es útil ser consciente de algunas diferencias.
Inyección de dependencias

Primero, como hemos visto previamente, los controladores de directiva deben usar anotaciones de inyección de dependencias para especificar que servicios necesitan, por ejemplo, $scope, $element, and $attrs. Una función enlazada siempre pasas los mismos cuatros parámetros scope, element, attrs, y controller, independientemente de los nombres de estos parámetros en la definición de la función.

El proceso de compilación

Los controladores de directivas y las funciones enlazadas son llamadas diferentes veces durante el proceso de compilación. Dado un conjunto de directivas en elementos DOM con esta estructura:

    

Los controladores de directiva y funciones enlazada son invocada en este orden: 

• parent (controller)
• parent (pre-link)
° child 1 (controller)
° child 1 (pre-link)
° child 1 a (controller)
° child 1 a (pre-link)
° child 1 a (post-link)
° child 1 b (controller)
° child 1 b (pre-link)
° child 1 b (post-link)
° child 1 (post-link)
• parent (post-link)

Si un elemento contiene múltiples directivas a continuación, para ese elemento:




Un ámbito es creado, si es necesario.
Cada controlador de directiva de cada directiva es instanciado.
Cada función de pre-enlace de directiva es llamada. 
Cualquier elemento hijo es enlazado. 
Cada función de post-enlace de directiva es llamada.

Esto significa que cuando un controlador de directiva es instanciado los elementos de la directiva y sus hijos no han sido completamente enlazados. Pero cuando la funciones de enlace (pre o post) son llamadas, todos los controladores de directiva para ese elemento ya han creado instancias. 
   
NOTA: La función post-enlace es llamada luego que el compilador ha terminado de compilar y enlazar el actual elemento y sus elementos hijos. Esto significa que cualquier cambio en el DOM en esta etapa no será notada por el compilador AngularJS.

Esto es útil cuando quieras enlazar librerías de terceros para elementos, tales como un plugin jQuery, el cual puede modificar el DOM en una forma que puede confundir al compilador de AngularJS.

Accediendo a otros controladores

Las funciones de enlace reciben cuatros parámetros que contienen cualquier controlador de directiva que han sido requerido (required:) por la directiva. Vimos cómo esto nos permite acceder a ngModelController en el Capítulo 9, Construyendo Directivas Avanzadas. 


myModule.directive('validateEquals', function() {
return {
require: 'ngModel',
link: function(scope, elm, attrs, ngModelCtrl) {
...
};
});

Aqui la directiva validateEquals esta requiriendo el controlador de directiva ngModel, el cual es entonces es pasado a la función de enlace como ngModelCtrl. 

En contraste, una directiva de controlador no puede tener otras directivas de controladores inyectada en ella. 




Accediendo a la función de transclución

Como se ha descrito en la sección de funciones de transclución, los controladores de directivas pueden ser inyectados con una función de $transclusion, la cual está ya unida al ámbito correcto. 

	Las funciones pueden solo acceder a las función de transclusion vía el closure de una función de compilación,  y esta función no está pre unida a un ámbito.   

	En esta sección nosotros observamos cómo implementar un widget de acordeón. Un widget de acordeón es una lista de grupos colapsables de contenido, con cabeceras cliciables. Hacer click en la cabecera de una grupo, lo expande y causa que los otros grupos colapsen. 




El HTML cuando se utiliza el accordion luce tal como:  


<accordion>
<accordion-group heading="Heading 1">
Group 1 <strong>Body</strong>
</accordion-group>
<accordion-group heading="Heading 2">
Group 2 <strong>Body</strong>
</accordion-group>
</accordion>

Aquí tenemos dos nuevos directivas de elementos: accordion, el cual es un contenedor para los grupos, y accordion-group,  el cual especifica el contenido de cada grupo. 

Usar un controlador de directiva en el acordeón 

Para permitir que los grupos comunicarse con cada otra directiva de accordion, defínes un controlador de directiva, AccordionController. Cada directiva accordion-group requerirá (require) este controlador.  

El controlador de directiva AccordionController, expondrá dos métodos, addGroup y closeOthers, que la directivas accordion-group usarán para registrarse como parte del acordeón y decir a los otros accordion-groups que cierren cuando una de estas sea abierta. 

Las pruebas unitarias en un controlador de directiva es muy similar a probar un controlador de la aplicación. Observa el capitulo 2, Construyendo y Probando, Una Aplicación de Ejemplo. Aqui una prueba para el método closeOthers: 

describe('closeOthers', function() {
var group1, group2, group3;
beforeEach(function() {
ctrl.addGroup(group1 = $scope.$new());
ctrl.addGroup(group2 = $scope.$new());
ctrl.addGroup(group3 = $scope.$new());
group1.isOpen = group2.isOpen = group3.isOpen = true;
});
it('closes all groups other than the one passed', function() {
ctrl.closeOthers(group2);
expect(group1.isOpen).toBe(false);
expect(group2.isOpen).toBe(true);
expect(group3.isOpen).toBe(false);
});
});


Añadimos tres grupos, los cuales todos están establecidos a isOpen=true. Luego de llamar closeOthers con group2, probamos que group1 y group2 tengan isOpen establecido como false. 

Aqui esta la implementación de AccordionController: 

myModule.controller('AccordionController', ['$scope', '$attrs',
function ($scope, $attrs) {
this.groups = [];
this.closeOthers = function(openGroup) {
angular.forEach(this.groups, function (group) {
if ( group !== openGroup ) {
group.isOpen = false;
}
});
}
};
this.addGroup = function(groupScope) {
var that = this;
this.groups.push(groupScope);
groupScope.$on('$destroy', function (event) {
that.removeGroup(groupScope);
});
};
this.removeGroup = function(group) {
var index = this.groups.indexOf(group);
if ( index !== -1 ) {
this.groups.splice(this.groups.indexOf(group), 1);
}
};
}]);

Nota que automáticamente removimos el grupo de la lista cuando su ámbito es destruido. Esto es importante porque nuestra lista de grupos puede generar dinámicamente a tiempo de ejecución un directiva ng-repeat, la cual puede remover elementos y así los ámbitos de grupo de la aplicación. Si nosotros mantenemos aun referencias de esos ámbitos de grupos, entonces ellos no serán elegibles por recolector de basura.  

Implementando la directiva de acordeón 

La directiva principal accordion solo especifica AccordionController como su controlador de directiva y añade la clase CSS accordion a su elemento en su función de enlace:

myModule.directive('accordion', function () {
return {
restrict:'E',
controller:'AccordionController',
link: function(scope, element, attrs) {
element.addClass('accordion');
}
};
})




Implementado la directiva accordion-group

Cada grupo colapsable será definido por la directiva accordionGroup. Cada grupo consta de un link y un body. Aqui esta la plantilla para esta directiva. 

<div class="accordion-group">
<div class="accordion-heading" >
<a class="accordion-toggle" ng-click="isOpen=!isOpen">{{heading}}</a>
</div>
<div class="accordion-body" ng-show="isOpen">
<div class="accordion-inner" ng-transclude></div>
</div>
</div>

Transcluimos los elementos hijos originales de la directiva dentro del cuerpo de la plantilla. La plantilla hace referencia de isOpen y heading, en el actual ámbito. Nosotros queremos tener un completo control sobre estos valores para que la directiva accordion-group tenga un ámbito aislado.    

En las pruebas, establecemos un accordion y algunas directivas accordion-group y luego verificamos que abran y cierren correctamente. Aqui esta un ejemplo de las pruebas unitarias: 

describe('accordion-group', function () {
var scope, element, groups;
beforeEach(inject(function($rootScope, $compile) {
scope = $rootScope;
var tpl =
"<accordion>" +
"<accordion-group heading='title 1'>Content 1</accordion-group>" +
"<accordion-group heading='title 2'>Content 2</accordion-group>" +
"</accordion>";
$compile(tpl)(scope);
scope.$digest();
groups = element.find('.accordion-group');
});
...
it('should change selected element on click', function () {
groups.eq(0).find('a').click();
expect(findGroupBody(0).scope().isOpen).toBe(true);
groups.eq(1).find('a').click();
expect(groups.eq(0).scope().isOpen).toBe(false);
expect(groups.eq(1).scope().isOpen).toBe(true);
});
...
});


Desencadenamos un evento click en los grupos y luegos verificamos que isOpen es false en el ámbito de los otros grupos. 

La implementación  de la directiva es bastante sencilla:

 myModule.directive('accordionGroup', function() {
return {
require:'^accordion',

restrict:'E',
transclude:true,
replace: true,
templateUrl:'template/accordion/accordion-group.html',
scope:{ heading:'@' },
link: function(scope, element, attrs, accordionCtrl) {
accordionCtrl.addGroup(scope);
scope.isOpen = false;
scope.$watch('isOpen', function(value) {
if ( value ) {
accordionCtrl.closeOthers(scope);
}
});
}
};
});

Puedes ver que esta directiva requiere el controlador de directiva de la directiva accordion que aparezca en un ancestro de elemento DOM de esta directiva. El requerido controlador de directiva aparece como cuarto parámetro, accordionCtrl, en el la función de enlace. La directiva accordion-group se registra a sí misma usando la función addGroup() y llama closeOthers() cada vez esta grupo es abierto. 
Tomando el control del proceso de compilación. 

Hay algunas situaciones donde necesitamos tener más control sobre cómo AngularJS compila y enlaza un elemento y sus hijos. Quizás necesitamos cargar la plantilla de la directiva de forma dinámica o queramos tener más control sobre la transclusion de los elementos dentro de la plantilla de directiva. En este caso nosotros podemos terminar el proceso de compilación, entonces modificar y compilar los elemento de la directiva y sus hijos manualmente. 

Crear una directiva de campo.

Cuando escribamos aplicaciones que usan formularios, rápidamente se hace evidente que hay una gran cantidad de duplicación y redundancia en la cantidad de texto HTML que requiere para cada campo en el formulario.

Por ejemplo, para cada campo, existirá un elemento imput y un elemento label rodeado por varios elementos span y div. En estos elementos, necesitamos proporcionar un números de atributos, ng-model, name, id, and for, los cuales son usualmente muy similares o incluso idénticos, y varias clases CSS. Nosotros también necesitamos desplegar mensajes de validación para cuando el valor de campo es inválido. 

Nosotros necesitamos terminar con esta tipo de HTML repetido en todos nuestros formularios: 

<div class="control-group" ng-class="{'error' : form.email.$invalid
&& form.email.$dirty, 'success' : form.email.$valid && form.
email.$dirty}">
<label for="email">E-mail</label>
<div class="controls">
<input type="email" id="email" name="email" ng-model="user.email"
required>
<span ng-show="form.email.$error['required'] && form.email.dirty"
class="help-inline">Email is required</span>
<span ng-show=" form.email.$error['email'] && form.email.dirty"
class="help-inline">Please enter a valid email</span>
</div>
</div>

Podemos eliminar mucho de esta duplicación creando una directiva de campo (field). La directiva insertará una plantilla adecuada que contiene control de entrada etiquetado. Aqui un ejemplo de usar la directiva field.

<field type="email" ng-model="user.email" required >
<label>Email</label>
<validator key="required">$fieldLabel is required</validator>
<validator key="email">Please enter a valid email</validator>
</field>

Ahora nosotro simplemente proporcionamos ng-model, type, y directivas de validación para el campo, como atributos en el campo. Entonces proporcionamos la etiqueta (label) y mensajes de validación como elementos hijos del elemento field. Nota, también que podemos usar la propiedad $fieldLabel dentro de los mensajes de validación. Esta propiedad será añadida a el ámbito de los mensajes de validación por la directiva field. 

La directiva field tiene un número de requerimientos que son difíciles de archivar con la API de directivas incorporada. 

En lugar de especificar una sola plantilla para toda la directiva, necesitamos insertar diferentes plantillas dependiendo del tipo de campo que será desplegado. No podemos usar la propiedad template (o templateUrl) en el objeto de definición de directiva. 

Necesitamos generar y aplicar atributos name e id únicos para el input y  cablear los elementos label por atributo, antes de que la directiva ng-model sea compilada. 

Queremos extraer los mensajes de validación para el campo de los elementos de validación hijos para ser usados en la plantilla cuando exista un error con el valor del campo. 


El objeto de definición de directiva para esta directiva de campo (field) tiene este aspecto:





restrict:'E',
priority: 100,
terminal: true,
compile: function(element, attrs) {
...
var validationMgs = getValidationValidationMessages(element);
var labelContent = getLabelContent(element);
element.html('');
return function postLink(scope, element, attrs) {
var template = attrs.template || 'input.html';
loadTemplate(template).then(function(templateElement) {
...
});
};
}


Finalizamos compilación en esta directiva, dando una prioridad (priority:) de 100, para asegurar que corra antes de la directiva ng-model que estará en el mismo elemento. En la función de compilación podemos extraer los mensajes de validación, getValidationMessageMap, y la información de la etiqueta, getLabelContent, del el elemento terminado. Una vez que esto ha sido recuperado, vaciamos el contenido del elemento para tener un elemento limpio con el cual cargar la plantilla. La función compile retorna una función postLink, la cual cargará una plantilla conveniente. 

Usando la propiedad terminal en directivas. 

Si la directiva tiene terminal: true, el compilador detendrá y no procesará los elementos hijos de esta directiva o cualquier otra directiva en esta directiva que tenga una baja prioridad que ésta directiva. 

NOTA: Incluso si terminas una directiva, el controlador de directiva, la función de compilación, y la función de enlace para tal directiva todavía son ejecutadas. 

Una vez que terminemos la compilación podemos modificar la directiva y sus hijos, pero entonces somos responsables por establecer cualquier ámbito nuevo, correctamente transcluir el contenido y también para las adicionales compilaciones de los elementos hijos que puedan contener directivas.  


La mayoría del tiempo AngularJS se encarga de interpolar cadenas en las expresiones, es decir, cuando se usa {{}} corchetes en plantillas. Pero en esta directiva necesitamos programáticamente interpolar una cadena. Esto es hecho con el servicio $interpolate. 

Usando el servicio $interpolate. 

La función getLabelContent simplemente copia todo el contenido HTML desde la etiqueta en la directiva hasta la etiqueta en la plantilla, donde será compilada junto con la plantilla:

   
function getLabelContent(element) {
var label = element.find('label');
return label[0] && label.html();
}

Pero para la los mensajes de validación, vamos a usar ng-repeat para desplegar cadenas solo para las validaciones que actualmente fallan. Así que vamos a necesitar almacenar los mensajes de validación en el ámbito de la plantilla, como $validationMessages. Estos mensajes de validación pueden contener cadenas interpoladas así que vamos a interpolar los durante la compilación:

function getValidationMessageMap(element) {
var messageFns = {};
var validators = element.find('validator');
angular.forEach(validators, function(validator) {
validator = angular.element(validator);
messageFns[validator.attr('key')] =
$interpolate(validator.text());
});
return messageFns;
}

Para cada uno de los elementos <validator> usando el servicio $interpolate para crear una función de interpolación del texto del elemento y añadir tal función dentro un mapa basado en el valor del atributo key del elemento validador. Este mapa será añadido a el ámbito de la plantilla como $validationMessages.

El servicio de interpolación es usado a través AngularJS para evaluar cadenas que contenga  {{}} llaves. Si pasamos tal cadena a el servicio, este retorna una función de interpolación que toma un ámbito y retorna una cadena interpolada. 


var getFullName = $interpolate('{{first}}{{last}}');
var scope = { first:'Pete',last:'Bacon Darwin' };
var fullName = getFullName(scope);


Aquí podemos crear una función de interpolación getFullName, de la cadena '{{first}}
{{last}}' y luego llamarla con un objeto scope, resultando en fullName sea asignado como 'Pete Bacon Darwin'.

Uniendo los mensajes de validación

Para desplegar los mensajes de error de validación en nuestras plantillas de campos, vamos a tener algo como esto:

<span class="help-inline" ng-repeat="error in $fieldErrors">
{{$validationMessages[error](this)}}
</span>

Estamos repitiendo sobre todas las claves de error de validación en $fieldErrors y uniendo el resultado de la llamada de la función de interpolación de validación para el clave del error dado.

Tenemos que proporcionar un ámbito a la función de interpolacion. Podemos hacer esto, en la plantilla, pasando this, el cual refiere al actual ámbito. No hacerlo puede dar lugar a un inesperado y un tiempo difícil en la depuración de errores.

La propiedad $fieldErrors contiene una lista de actuales inválidas llaves (keys) de los errores de validación. Se actualiza por un observador creado en el manejador success para loadTemplate() .

Cargar plantillas dinámicamente

La función loadTemplate carga la plantilla especificada y la convierte a una jqLite/jQuery envuelta elemento DOM:

function loadTemplate(template) {
return $http.get(template, {cache:$templateCache})
.then(function(response) {
return angular.element(response.data);
}, function(response) {
throw new Error('Template not found: ' + template);
});
}
La función es asíncrona y por lo que retorna una promesa al elemento de envuelto. Justo como hace ng-include y templateUrl usando directivas, estamos usando $templateCache para almacenar en cache las plantillas cuando las cargemos. 


Estableciendo la plantilla del campo

En la función de enlace (link) directiva field, llamamos loadTemplate con el valor del atributo de la plantilla  en la directiva (o 'input.html' si ninguna es especificada).

loadTemplate(template).then(function(templateElement)

Todo el trabajo de la directiva sucede una vez que la promesa es resuelta. 

var childScope = scope.$new();
childScope.$validationMessages = angular.copy(validationMsgs);
childScope.$fieldId = attrs.ngModel.replace('.', '_').toLowerCase()
+ '_' + childScope.$id;
childScope.$fieldLabel = labelContent;
childScope.$watch('$field.$dirty && $field.$error',
function(errorList) {
childScope.$fieldErrors = [];
angular.forEach(errorList, function(invalid, key) {
if ( invalid ) {
childScope.$fieldErrors.push(key);
}
});
}, true);


Primero creamos un nuevo ámbito hijo y anexamos propiedades útiles, tales como $validationMessages , $fieldId , $fieldLabel , y $fieldErrors :


var inputElement = findInputElement(templateElement);
angular.forEach(attrs.$attr, function (original, normalized) {
var value = element.attr(original);
inputElement.attr(original, value);
});
inputElement.attr('name', childScope.$fieldId);
inputElement.attr('id', childScope.$fieldId);

Copiamos todos los atribustos del la directiva campo a la plantilla del elemento input y añadimos los valores computados para los atributos id y name:

var labelElement = templateElement.find('label');
labelElement.attr('for', childScope.$fieldId);
labelElement.html(labelContent);


Lo copiamos en labelContent y aplicamos el atributo for para el elemento label:

element.append(templateElement);
$compile(templateElement)(childScope);
childScope.$field = inputElement.controller('ngModel');
});


Anexamos el templateElement a el original elemento field, entonces usamos el servicio $compile para compilar y enlazar a nuestro nuevo ámbito hijo. Una vez que el elemento es enlazado, el  ngModelController esta disponible para nosotros introducirlo en la propiedad $field para que la plantilla lo use. 

Resumen

En este capítulo hemos visto algunos de los aspectos más avanzados de desarrollar directivas. Vimos en nuestra directiva alert, como la transclución puede ser usada con  ng-transclude cuando se crean widgets. La directiva acordeon prolijamente demostró como las directivas controladoras pueden ser usadas para coordinar comunicación entre directivas. Incluso tomamos total control del proceso de compilación terminado la actual compilación en la directiva field y usamos el servicio para manualmente compilar el contenido de los elementos. 

En el próximo capítulo estaremos observando que podemos hacer para asegurar que nuestra aplicación se desempeñe rápido y responsable. 



270 -271


