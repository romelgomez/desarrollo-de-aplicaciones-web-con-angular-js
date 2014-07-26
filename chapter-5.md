## Capítulo 5. Creando formularios avanzados

AngularJS construye sobre los formularios HTML estándar y elementos de entrada de datos (imput). Esto significa que puede continuar creando su interfaz de usuario con los mismos elementos HTML que ya entiende usando las herramientas de diseño HTML estándar.  

Hasta ahora, en nuestra aplicación SCRUM, hemos creado algunos formularios básicos con elementos de entrada de datos unidos a modelo de datos y botones para guardar y borrar, etc. AngularJS se encarga de efectuar la conexión, tanto la unión elemento-modelo y la unión evento-controlador.

En este capítulo, veremos como los formularios AngularJS trabaja en detalle y luego como añadir validación y interacción de usuario dinámica a nuestros formularios de aplicaciones.

En este capítulo vamos a cubrir:       

- Unión de datos del modelo y directivas de campos de entrada.
- Validación de formularios.
- Formularios anidados y repetidos.
- Envío de formularios.
- Restablecer de un formulario.

### Comparando formularios tradicionales con formularios AngularJS

Antes de comenzar a mejorar nuestros formularios de aplicaciones debemos entender como trabaja los formularios AngularJS. En esta sección, explicamos las diferencias entre los elementos estándares de entrada de datos HTML y las directivas de entrada de datos AngularJS. Nosotros mostraremos como AngularJS modifica y expande el comportamiento de los elementos de entrada de datos HTML y la forma en que administra las actualizaciones desde y hacia el modelo para usted.

En un formulario HTML, el valor de un elemento input es el valor que se enviará al servidor al enviar el formulario.
 
![chapter-5-1](/img/chapter-5-1.png)

El problema con los elementos de entrada de datos (input) que contiene el valor de su envío es que usted está atascado en tener que trabajar con los valores de los elementos de entrada de datos a medida que se muestran al usuario. A menudo, esto no es lo que quieres. Por ejemplo, un campo de entrada del tipo fecha (date) puede permitir que el usuario introduzca un cadena en algún formato predefinido, por ejemplo "12 March 2013". Pero en su código puede que quiera trabajar con el valor como un objeto JavaScript Date. La constante codificación de estas transformaciones es tediosa y propensa a errores.

AngularJS desacopla el modelo de la vista. Dejas que las directivas de elementos de entrada de datos se preocupen por desplegar los valores y AngularJS se preocupe por actualizar su modelo cuando el valor cambie. Esto lo deja libre para trabajar con el modelo, via controlador por ejemplo, sin preocuparse sobre cómo los datos son desplegados o ingresados.

![chapter-5-2](/img/chapter-5-2.png)

Para lograr esta separación, AngularJS mejora los formularios HTML con las directivas form y input, directivas de validación, y controladores, Estas directivas y controladores reescriben el comportamiento integrado de los formularios HTML pero, para el observador casual, los formularios en AngularJS lucen muy similares a los formularios HTML estándar.

En primer lugar, la directiva `ngModel` permite definir cómo los elementos de entrada de datos deben unirse al modelo.
      
### Presentado la directiva ngModel

Ya hemos visto como AngularJS crea el enlace de datos entre el objeto ámbito y los elementos HTML en la página. Se puede configurar el enlace de datos usando llaves, `{{}}`, o directivas tal como `ngBind`. Pero el uso de tales enlaces es solo en una via. Al enlazar el valor de una directiva input, usamos `ngModel`:

```
<div>Hello <span ng-bind="name"/></div>
<div>Hello <input ng-model="name"/></div>
```

Pruebelo en: http://bit.ly/Zm55zM.

En el primer div, AngularJS une `scope.name` del actual ambito al texto del elemento `span`. Esta unión es direccional: Si el valor de `scope.name` cambia, el texto del elemento span cambia; pero si nosotros cambiamos el texto del elemento span, el valor de `scope.name` no cambiara.     

En el segundo div, AngularJS une `scope.name` del actual ambito al valor del elemento `input`. Aquí el enlace de datos es realmente bidireccional, ya que si modificamos el valor del input escribiendo en él, el valor del modelo `scope.name` es instantaneamente actualizado. Esta actualización de `scope.name` se ve luego en el enlace direccional al `span`.

![chapter-5-3](/img/chapter-5-3.png)

> `NOTA` ¿Por qué tenemos una directiva diferente para especificar la unión en inputs? La directiva `ngBind` sólo enlaza (direccionalmente) el valor de su expresión al texto del elemento. Con `ngModel`, el enlace de datos es bidireccional, por lo que los cambios del valor del input se reflejan en el modelo.

Además, AngularJS permite a las directivas transformar y validar los valores de la directiva `ngModel` como el enlace de datos se sincroniza entre el modelo y la directiva input. Verás como esto trabaja en la sección `ngModelController`.
 
## Creación un Formulario de Información de usuario.

En esta sección describiremos un simple formulario de información de usuario de nuestro ejemplo de aplicación SCRUM. A lo largo de este capítulo de forma incremental añadiremos funcionalidades a este formulario para demostrar el poder de los formularios AngularJS. Aquí está nuestro básico formulario funcional:

```
<h1>User Info</h1>
<label>E-mail</label>
<input type="email" ng-model="user.email">

<label>Last name</label>
<input type="text" ng-model="user.lastName">

<label>First name</label>
<input type="text" ng-model="user.firstName">

<label>Website</label>
<input type="url" ng-model="user.website">

<label>Description</label>
<textarea ng-model="user.description"></textarea>

<label>Password</label>
<input type="password" ng-model="user.password">

<label>Password (repeat)</label>
<input type="password" ng-model="repeatPassword">

<label>Roles</label>
<label class="checkbox">
<input type="checkbox" ng-model="user.admin"> Is Administrator
</label>

<pre ng-bind="user | json"></pre>
```

Pruebelo en: http://bit.ly/10ZomqS.

Aunque parece que nosotros simplemente tenemos un lista de imputs HTML estándar, estos son en realidad directivas `input` AngularJS. A cada input se le ha dado una directiva `ngModel` que define que actual ámbito se unirá al valor del elemento `input`. En este caso, cada input es unido a un campo en el objeto user, que a su vez está unido al actual ámbito. En un controlador podemos registrar el valor de un campo del modelo de este modo:

```
$log($scope.user.firstName);
```

Nótese que no hemos utilizado un elemento `form` o colocado atributos `name` o `id` en cualquiera de los elementos `input`. Para simples formularios sin validaciones esto es todo lo que necesitamos. AngularJS se asegura de que los valores de los elementos input estén sincronizados con los valores en el modelo. Estamos entonces libres para trabajar con el modelo de usuario en un controlador, por ejemplo, sin preocuparse de cómo se representan en la vista.

> `NOTA` También agregamos un elemento `pre` con una representación JSON del modelo usuario. Esto es para que podamos ver lo que está siendo sincronizando al modelo por AngularJS

## Comprendiendo las directivas input

En esta sección describimos las directivas input AngularJS que se proporcionan fuera de la caja. Usar las directivas input es muy natural para las personas que usan los formularios HTML porque AngularJS construye sobre el HTML.

Puedes usar todos los tipos de input HTML estándar en sus formularios. La directivas `input` trabajan con la directiva `ngModel` para soportar funcionalidades adicionales, tal como la validación o la unión al modelo. La directiva `input` AngularJS comprueba el tipo de atributos para identificar que tipo de funcionalidad añadir al elemento `input`.

### Añadiendo la validación requerida

Todas las directivas input básicas soportan el uso del atributo `required` (o `ngRequired`). Añadiendo este atributo a su elemento `input` le dirá a AngularJS que el valor de `ngModel` es inválido si es `null`, `undefined`, o `""` (la cadena vacía). Consulte la siguiente sección sobre validación de campos para más información sobre esto.

### Usando inputs basados en texto (text, textarea, e-mail,URL, number)

La directiva `input` básica, `type="text"` o `textarea`, acepta cualquier cadena para su valor. Cuando cambias el texto de el `input`, el modelo es instantáneamente actualizado con el valor.  

La otra directiva `input` basada en texto, tal como `e-mail`, `URL`, o `number`, actúa similarmente excepto que solo permite que el modelo se actualice si el valor en el input concuerda con la apropiada expresión regular. Si escribes dentro de `input` e-mail, el campo e-mail en el modelo está en blanco hasta que el input contenga una cadena e-mail válida. Esto significa que su modelo nunca contendrá una dirección de e-mail invalida. Este es uno de los beneficios de desacoplar el modelo de la vista.

En adición a estas validaciones todas las directivas basadas en texto le permiten especificar un mínimo y un máximo de longitud para el texto así como también una arbitraria expresión regular que deba coincidir. Esto se hace con la directivas `ngMinLength`, `ngMaxLength`, y `ngPattern`:

```
<input type="password" ng-model="user.password" ng-minlength="3" ng-maxlength="10" ng-pattern="/^.*(?=.*\d)(?=.*[a-zA-Z]).*$/">
```

Pruebelo en: http://bit.ly/153L87Q.

Aquí, el campo de modelo `user.password` debe tener entre 3 y 10 caracteres, inclusive, debe coincidir con una expresión regular que le obliga a incluir al menos una letra y un número.

> `NOTA` Tenga en cuenta que estas funciones de validación incorporadas no dejan que el usuario ingrese una cadena no válida. La directiva simplemente borra el campo de modelo si la cadena es inválida.

### Usando inputs checkbox

Checkboxes simplemente indican un input booleano. En nuestro formulario, la directiva input asigna `true` o `false` a el campo de modelo que es especificado por `ngModel`. Puede ver esto sucediendo en nuestro formulario de información de usuario para el campo "Es administrador".

```
<input type="checkbox" ng-model="user.admin">
```

El valor de `user.admin` es establecido a true si el checkbox es comprobado y `false` de lo contrario. Por el contrario, el checkbox será marcado si el valor de `user.admin` es `true`.

Puedes también especificar diferentes cadena para valores `true` y `false` para ser usado en el modelo. Por ejemplo, podemos usar `admin` y `basic` en un campo de rol.

```
<input type="checkbox" ng-model="user.role" ng-true-value="admin" ngfalse-value="basic">
```

Pruebelo en: http://bit.ly/Yidt37.

En esta caso, el modelo `user.role`, puede contener cualquiera: `admin` o `basic` dependiendo de que si el checkbox a sido marcado o no.

### Usando inputs radio.

Los botones de radio proporcionan un grupo fijo de opciones para un campo. AngularJS hace esto realmente simple de implementar: Solo une todos los botones radio en un grupo para el mismo campo de modelo. El atributo HTML estándar value es entonces usado para especificar que valor colocar en el modelo cuando el radio es seleccionado:

```
<label><input type="radio" ng-model="user.sex" value="male"> Male</label>
<label><input type="radio" ng-model="user.sex" value="female">Female</label>
```

Pruebelo en: http://bit.ly/14hYNsN.

### Usando inputs select

La directiva input select te permite crear una lista desplegable, a partir del cual el usuario pueda seleccionar uno o más elementos. AngularJS te deja especificar opciones para el desplegable estáticamente o desde una matriz en el ámbito.  

### Proporcionando simples opciones de cadena.

Si tienes una lista estática de opciones entre las que elegir puedes simplemente proporcionar como elementos option abajo el elemento select:

```
<select ng-model="sex">
  <option value="m" ng-selected="sex=='m'">Male</option>
  <option value="f" ng-selected="sex=='f'">Female</option>
</select>
```

Tenga en cuenta que, dado que el atributo value puede solo tomar una cadena, el valor al que se va unir sólo puede ser una cadena.

> `NOTA` Si quiere enlazar a valores que no son cadenas o quiere su lista de opciones sean creada dinámicamente a partir de los datos, entonces use `ngOption` de la siguiente manera.  

### Proporcionando opciones dinámicas con la directiva ngOptions  

AngularJS proporciona una sintaxis adicional para definir dinámicamente una lista compleja de opciones para una directiva `select`. Si quieres enlazar el valor de una directiva `select` a un objeto, en lugar de una simple cadena, entonces usa `ngOptions`. Este atributo acepta una expresión de compresión que define qué opciones se han de visualizar. La forma de esta expresión es:

![chapter-5-4](/img/chapter-5-4.png)

La expresión `dataSource` describe la fuente de la información acerca de las opciones que se mostrarán. En él se describe elementos en una matriz o propiedades en un objeto. Una opción de selección se generará para cada elemento de la expresión `dataSource`.

La expresión `optionBinding` describe que debe ser extraído de cada elemento de origen de datos y cómo ese elemento debe estar unido a la opción de selección.

### Ejemplos comunes de ngOptions

Antes de explicar los detalles de cómo definir estas **expresiones de compresión**, Aquí están algunos ejemplos típicos.  

#### Usando matrices como fuente de datos  

Seleccione un objeto user con `user.email` como una etiqueta:

```
ng-options="user.email for user in users"
```

Seleccione un objeto user con una etiqueta computarizada (la función debe ser definida en el ámbito):  

```
ng-options="getFullName(user) for user in users"
```

Selecciona un e-mail del usuario en lugar de todo el objeto user, con su nombre completo como etiqueta.

```
ng-options="user.email as getFullName(user)for user in users"
```

Selecciona un objeto user con la lista agrupados por sex:

```
ng-options="getFullName(user) group by user.sex for user in users"
```

Pruebelo en:  http://bit.ly/1157jqa.

#### Usando objeto como fuente de datos.

Vamos a proporcionar dos objetos que se relacionan los nombres de países con códigos:

```
$scope.countriesByCode = {
'AF' : 'AFGHANISTAN',
'AX' : 'ÅLAND ISLANDS',
...
};

$scope.countriesByName = {
'AFGHANISTAN' : 'AF',
'ÅLAND ISLANDS' : 'AX',
...
};
```

Para seleccionar un código de un país por el nombre del país, ordenado por código del país.

```
ng-options="code as name for (code, name) in countriesByCode"
```

Para seleccionar un código de un país por el nombre del país, ordenado por el nombre del país.

```
ng-options="code as name for (name, code) in countriesByName"
```

Pruébalo en: http://bit.ly/153LKdE

Ahora que hemos visto algunos ejemplos, podemos mostrar la full especificación de estas expresiones.

### Entendiendo la expresiones de dataSource

Si la fuente de datos es un array entonces la expresión `arrayExpression` debe evaluar un array. La directiva va iterar sobre cada elemento del array, asignado el actual elemento en el array a la variable `value`.   

> `NOTA` la lista de opciones seleccionadas serán desplegadas en el mismo orden como los elementos aparecen en el array.

Si la fuente de datos es un objeto entonces la expresión `objectExpression` debe evaluar un objeto. La directiva va iterar sobre cada propiedad del objeto, asignando el valor de la propiedad a la variable `value` y el `key` del elemento a la variable `key`.

> `NOTA` La lista de opciones de selección será ordenada alfabéticamente por el valor del key.

### Entendiendo la expreción optionBinding.

La expresión `optionBinding` define como obtener la etiqueta y valor para cada opción y como agrupar las opciones de los elementos proporcionados por la expresión `dataSource`. Esta expresión puede tomar ventaja de toda las sintaxis de expresiones AngularJS, incluyendo el uso de filtros. La sintaxis general es:    

```
value as label group by grouping
```

Si la expresión `value` no es proporcionada entonces los datos del elemento se usarán como valor para asignar a el modelo cuando este elemento es seleccionado.  Si suministras una expresión de agrupamiento, debería evaluar a el nombre de el grupo para la opción dada.

### Usando opciones vacías con la directiva select.

¿Que debe hacer la directiva `select` cuando el valor de modelo unido no concuerda con cualquiera de los valores en la lista de opciones? En este caso, la directiva `select` mostrará una opción vacía al inicio de la lista de opciones.

> `NOTA` La opción vacía será seleccionada siempre que el modelo no concuerde con cualquiera de las opciones. Si el usuario manualmente selecciona una opción vacía entonces el modelo será establecido null. No se establecerá undefined.

Puede definir una opción vacía añadiendo un elemento `option` como hijo del elemento `select` que tenga un cadena vacía para su valor:      

```
<select ng-model="..." ng-options="...">
  <option value="">-- No Selection --</option>
</select>
```

Pruébalo en: http://bit.ly/ZeNpZX

Aquí definimos una opción vacía, que desplegará la etiqueta -- No Selection --.

> `NOTA` Si define su propia opción vacía entonces esta será siempre mostrada en la lista de opciones y puede ser seleccionada por el usuario.

Si no defines su propia opción vacía en la declaración de la directiva `select` esta generará su propia.    

> `NOTA` Si la directiva genera la opción vacía, esta será mostrada solo cuando el modelo no concuerde con cualquier elemento en la lista. Así el usuario no podrá manualmente establecer el valor del `select` a `null/undefined`.

Es posible esconder la opción vacía definiendo su propia y estableciendo su estilo a `display: none`.

```
<option style="display:none" value=""></option>
```

Pruebelo en: http://bit.ly/ZeNpZX

En este caso la directiva `select` usará nuestra opción vacía pero el navegador no la mostrará. Ahora, si el modelo no concuerda con cualquiera de la opciones la directiva `select` estará en blanco y será no válida, pero no será mostrada una opción en blanco en la lista.

### Entendiendo select y el objeto de equivalencia.

La directiva `select` encuentra el valor del modelo para el valor option usando el operador de equivalencia de objeto (`===`). Esto significa que si sus valores de option son objeto y no simples valores (como números y cadenas) debes usar una referencia para el actual valor de option para su valor de modelo. De otra manera la directiva `select` pesara que los objetos son diferentes y no coincidirá con la opción.

En un controlador podríamos establecer las opciones y elementos seleccionados como un array de objetos:

```
app.controller('MainCtrl', function($scope) {
   $scope.sourceList = [
       {'id': '10005', 'name': "Anne"},
       {'id': '10006', 'name': "Brian"},
       {'id': '10007', 'name': "Charlie"}
   ];
   $scope.selectedItemExact = $scope.sourceList[0];
   $scope.selectedItemSimilar = {'id': '10005', 'name': "Anne"};
});
```

Aqui, `selectedItemExact` actualmente referencia el primer elemento en la `sourceList`, mientras que `selectedItemSimilar` es un objeto diferente, aunque los campos son idénticos.

```
<select
  ng-model="selectedItemExact"
  ng-options=" item.name for item in sourceList">
</select>
<select
  ng-model="selectedItemSimilar"
  ng-options="item.name for item in sourceList">
</select>
```

Pruébalo en: http://bit.ly/Zrachk

Aquí, nosotros creamos dos directivas `select` que están unidas a esos valores. La primera está unida a `selectedItemSimilar` no tendra una opcion seleccionada. Por lo tanto, debes unir siempre el valor del `select` a un elemento en la matriz `ng-options`. Es posible que tenga que buscar la matriz para la opción apropiada.   

### Seleccionando múltiples opciones.

Si quiere seleccionar múltiples elementos, simplemente aplique el atributo multiple a la directiva `select`. El `ngModel` unida a esta directiva es entonces una matriz que contiene una referencia a el valor de cada opción seleccionada.

> `NOTA` AngularJS provee la directiva `ngMultiple`, la cual toma una expresión para decidir cuándo permitir múltiples selecciones. Actualmente la directiva select no observa los cambios cuando acepta múltiples selecciones, por lo que la directiva `ngMultiple` tiene un uso limitado.

### Trabajando con campos de entrada (inputs) ocultos HTML tradicionales

En AngularJS, almacenamos todo nuestros datos del modelo en el ámbito, de modo que es raramente necesario usar campos de entrada ocultos. Por lo tanto, AngularJS no tiene una directiva de campos de entrada ocultos. Hay dos casos donde puedas querer usar campos de entrada ocultos: valores incorporados desde el servidor y soportar el envíos de formularios HTML tradicionales.

### Incorporando valores desde el servidor.  

Usas un motor de plantillas del lado del servidor para crear HTML y pasas datos desde el servidor a AngularJS vía la plantilla. En este caso, es suficiente colocar la directiva ng-init dentro el HTML que es generado por el servidor, que añadirá valores al ámbito:

```
<form ng-init="user.hash='13513516'">
```

Aqui el HTML enviado por el servidor contiene un elemento form que incluye un directiva `ng-init` que inicializará `user.hash` en el ámbito del formulario.

### Enviando un formulario HTML tradicional.

Tradicionalmente, usted podría haber querido enviar valores al servidor que no están en la vista, es decir, no es un control de entrada visible (input visible). Esto se habría logrado mediante la adición de campos ocultos a su formulario. En AngularJS, trabajamos a partir de un modelo que está desacoplado del formulario, por lo que no necesitamos esos campos ocultos. Nosotros simplemente añadimos tales valores al ámbito y luego simulamos el envío del formulario usando el servicio `$http`. Ve el Capítulo 3, comunicación con el servidor Back-end para saber cómo hacerlo.  

## Mirando dentro del enlace de datos de ngModel

Hasta ahora hemos visto que `ngModel` crea una unión entre el modelo y el valor en un campo de entrada. En esta sección vemos en profundidad en lo demás que esta directiva proporciona y cómo funciona.                                          

### Entendiendo ngModelController

Cada directiva `ngModel` crea una instancia de ngModelController. Este controlador está a disposición de todas las directivas en el elemento `input`.

![chapter-5-5](/img/chapter-5-5.png)

El `ngModelController` es responsable de administrar la unión de datos entre el valor almacenado en el modelo (especificado por `ngModel`) y el valor desplegado por el elemento input.

El controlador `ngModelController` también realiza un seguimiento de si el valor de la vista es válido y si ha sido modificado por el elemento input.

### Transformando el valor entre el modelo y la vista

El controlador `ngModelController` tiene una línea de transformación que es aplicada cada vez que los datos son actualizados. Esto consta de dos matrices: `$formatters` que transforma del modelo a la vista y `$parsers` que transforma de la vista al modelo. Cada directiva en el elemento input puede agregar sus propios formateadores y analizadores para esta línea en orden de modificar lo que pasa en la unión de datos como se muestra en la siguiente imagen:

![chapter-5-6](/img/chapter-5-6.png)

Aqui dos directivas son añadidas a la línea de transformación. La directiva `date` esta analizando y formateando fechas. La directiva `ng-required` está comprobado que el valor no esté faltando.  

### Seguimiento de si el valor ha cambiado

Además de transformar el valor entre el modelo y la vista, el controlador `ngModelController` sigue si el valor ha cambiado desde que fue inicializado y si el valor es válido.  

Cuando es inicializado primero el controlador `ngModelController` marca el valor como original (pristine), es decir, no ha sido modificado. Este se expone como una clase CSS `ng-pristine` en el elemento input. Cuando la vista cambia, al escribir en un campo `input`, el valor es marcado como sucio. Reemplaza la clase `ng-pristine` con la clase `ng-dirty`.    

Proporcionado estilos CSS para estas clases, podemos cambiar la apariencia de un elemento input basado en si el usuario ha introducido o modificado los datos.

```
.ng-pristine { border: solid black 1px; }
.ng-dirty { border: solid black 3px; }
```

Aquí hacemos el borde del elemento más grueso si el usuario hace cambios en el input.

### Seguimiento a la validez de un campo de entrada

Las directivas en un elemento input pueden decir también al controlador `ngModelController` si creen que valor es valido o invalido. Esto se hace normalmente mediante la conexión a la línea de transformación y confirmación del valor en lugar de transformándolo. El controlador `ngModelController` sigue la validez y aplica en consecuencia la clase CSS `ng-invalid` o `ng-valid`. Podemos proporcionar más estilos para cambiar la apariencia del elemento basado en estas clases:

```
.ng-valid.ng-dirty { border: solid green 3px; }
.ng-invalid.ng-dirty { border: solid red 3px; }
```

Aquí, podemos usar la combinación de original (pristine) y inválida para asegurarnos que solo el campo ha sido cambiado por el usuario es estilizado: un grueso borde rojo cuando es inválido y un grueso borde verde cuando es inválido.

En la próxima sesión. Validando formularios, veremos cómo podemos trabajar con el concepto de original (pristine), sucio (dirty), válido  (valid), y inválido (invalid) programáticamente.

## Validando formularios AngularJS

En esta sección vamos a explicar como usar directivas de validación y como funciona con el controlador `ngFormController` para proporcionar un marco de validación completo.

### Entendiendo el controlador ngFormController

Cada directiva form (o `ngForm`) crea una instancia del controlador `ngFormController`. El objeto `ngFormController` administra si el formularios es valido o invalido y si este es original o sucio. Importante, este funciona con el controlador `ngModelController` para seguir cada campo `ngModel` dentro del formulario.

Cuando un controlador `ngModelController` es creado, se registra a sí mismo con el primer controlador `ngFormController` que viene atravez como atraviesa su lista de elementos padres. De esta forma, el controlador ngFormController conoce que directiva input debe hacerle seguimiento. Esta puede comprobar si alguno de estos campos son válidos/inválidos o original/sucio y establecer en consecuencia si el formulario es valido/invalido o original/sucio.

### Usando el atributo name para vincular formularios al ámbito.

Puedes hacer que el controlador `ngFromController` aparezca en el ámbito local dándole al formulario un nombre. Cualquier elemento input dentro del formulario que también tenga nombres tendrán su objeto `ngModelController` adjuntado como una propiedad a este objeto `ngFormController`.

La siguiente tabla muestra como el ámbito contiene el controlador asociado con cada elemento en el formulario.  

![chapter-5-7](/img/chapter-5-7.png)

### Añadiendo comportamiento dinámico al formulario de información de usuario.

Nuestro formulario nos permite ingresar valores dentro de campos y podemos cambiar la apariencia de los elementos input basados en los valores ingresados. Pero para una más responsable experiencia de usuario, nos gustaría mostrar y ocultar los mensajes de validación y cambiar el estado de los botones en nuestro formulario dependiendo del estado del los campos del formulario.

Teniendo los objetos ngFormController y  ngModelControllers en nuestro ámbito nos permite trabajar con el estado del formulario programáticamente. Podemos usar valores tales como $invalid y $dirty para cambiar que es habilitado o visible al nuestro usuario.

### Mostrando errores de validación

Podemos mostrar mensajes de error para los campos y para el formulario como un todo si algo no es válido. En la plantilla:  

```
<form name="userInfoForm">
   <div class="control-group"
        ng-class="getCssClasses(userInfoForm.email)">
       <label>E-mail</label>
       <input type="email" ng-model="user.email"
              name="email" required>
       <span ng-show="showError(userInfoForm.email, 'email')" ...>
       You must enter a valid email
       </span>
       <span ng-show="showError(userInfoForm.email, 'required')" ...>
       This field is required
       </span>
   </div>
   ...
</form>
```

En el controlador:

```
app.controller('MainCtrl', function($scope) {
   $scope.getCssClasses = function(ngModelController) {
       return {
           error: ngModelController.$invalid && ngModelController.$dirty,
           success: ngModelController.$valid && ngModelController.$dirty
       };
   };
   $scope.showError = function(ngModelController, error) {
       return ngModelController.$error[error];
   };
});
```

Pruébalo en: http://bit.ly/XwLUFZ

Este ejemplo muestra el input e-mail para nuestro formulario de usuario (User Form). Estamos usando Twitter Bootstrap CSS para estilizar el formulario, de ahí las clases CSS control-group y inline-help. También hemos creado dos funciones ayudantes (helper) en el controlador.

La directiva ng-class actualizará las clases CSS en el div que contiene la etiqueta, el input, y el texto de ayuda. Llama el método getCssClasses(), pasando un objeto y un nombre de error.   

> `NOTA` El parámetro objeto es actualmente es el ngModelController, que ha sido expuesto en el ngFormController, que a su vez se expone en el ámbito scope.userInfoForm.email.

El método getCssClasses() retorna un objeto que define que clase CSS debe ser añadida. La llave de cada objeto se refiere al nombre de una clase CSS. El valor de cada miembro es true si la clase es añadida. En este caso  getCssClasses() retornará error si el modelo es sucio y invalido y success si el modelo es sucio y válido.

### Deshabilitando el botón de guardado.

Podemos deshabilitar el botón save, cuando el formulario no esta un estado para ser guardado.

```
<form name="userInfoForm">
   ...
   <button ng-disabled="!canSave()">Save</button>
</form>
```

En nuestra vista, añadimos un botón de Save con una directiva ngDisabled, Esta directiva deshabilitará el botón siempre que su expresión se evalúe como verdadera. En este caso está negando el resultado de llamar al método canSave(). Proporcionamos el método canSave() en el ámbito actual. Lo haremos en nuestro controlador principal.

```
app.controller('MainCtrl', function($scope) {
   $scope.canSave = function() {
       return $scope.userInfoForm.$dirty &&
           $scope.userInfoForm.$valid;
   };
});
```

Pruébalo en: http://bit.ly/123zIhw

El método canSave() comprueba si el usuario userInfoForm tiene la bandera $valid y $dirty establecida. Si es así, el formulario está listo para ser guardado.

### Deshabilitando la validación nativa del navegador

Los navegadores modernos naturalmente tratan de validar el valor del input en un formulario. Normalmente esto ocurre cuando el formulario es enviado. Por ejemplo, si tienes un atributo required en un campo input, el navegador se quejara de forma independiente de AngularJS, si el campo no contiene un valor cuando tratas de enviar el formulario.

Ya que estamos proporcionado toda la validación a través de las directivas AngularJS y controladores, no queremos que el navegador intente su propia validación nativa. Podemos desactivar esto aplicando el atributo novalidate HTML5 a el elemento form:

```
<form name="novalidateForm" novalidate>
```

Pruébalo en: http://bit.ly/1110hS4

Este formulario es llamado novalidateForm y el atributo novalidate le dirá al navegador que no intente la validación en cualquier inputs del formulario.

## Formularios Anidados en otros Formularios

A diferencia de los formularios HTML estándar, los formularios AngularJS pueden ser anidados uno dentro del otro. Ya que las etiquetas form dentro de otras etiquetas form es HTML inválido, AngularJS provee la directiva ngForm para anidar formularios.

> `NOTA` Cada formulario que proporcione un nombre (name) será añadido a su formulario padre, o directiva en el ámbito sí no tiene un formulario padre.

### Usando Sub-Formularios como componentes reusables

Un formulario anidado actúa como un campo compuesto que expone su propia información de validación basado en los campos que esta contenga.  Tal que los formularios puedan ser usados y reusados como subformularios mediante su inclusión en contenedores de formularios. Aquí agrupamos dos cajas inputs para crear un widget de contraseña y confirmación de contraseña:

```
 <script type="text/ng-template" id="password-form">
   <ng-form name="passwordForm">
   
     <div ng-show="user.password != user.password2">
       Passwords do not match
     </div>
   
     <label>Password</label>
     <input ng-model="user.password" type="password" required>

     <label>Confirm Password</label>
     <input ng-model="user.password2" type="password" required>
   
   </ng-form>
 </script>
 
 <form name="form1" novalidate>
   <legend>User Form</legend>
   <label>Name</label>
   <input ng-model="user.name" required>
   <ng-include src="'password-form'"></ng-include>
 </form>
```

Pruebalo en: http://bit.ly/10QWwyu

Definimos nuestro subformulario en una plantilla parcial. En este caso él está en línea en un bloque de script pero podría estar en un archivo también. Luego tenemos nuestro contenedor de formulario, form1, el cual incluye el subformulario usando la directiva ngInclude.

El subformulario tiene su propio estado de validación y clases CSS relacionadas. También nota que, debido a que el subformularios tiene un atributo name, este aparece como una propiedad en el formulario contenedor.

## Repitiendo subformularios.   

Algunas veces, tenemos campos en un formulario que necesitan ser repetidos un arbitrario número de veces basados en los datos en el modelo. Esta es una situación común en la que desea proporcionar un solo formulario que pueda representar una relación de uno-a-muchos en los datos.

En nuestra aplicación SCRUM, nos gustaría permitir a los usuarios tener cero o más URLs de sitios web en su perfil de información de usuario.  Podemos usar una directiva ngRepeat para establecer esto:   

```
 <form ng-controller="MainCtrl">
     <h1>User Info</h1>
     <label>Websites</label>
     <div ng-repeat="website in user.websites">
         <input type="url" ng-model="website.url">
         <button ng-click="remove($index)">X</button>
     </div>
     <button ng-click="add()">Add Website</button>
 </form>
```

El controlador inicializa el modelo y proporciona las funciones ayudantes, remove() y add():

```
app.controller('MainCtrl', function($scope) {
   $scope.user = {
       websites: [
           {url: 'http://www.bloggs.com'},
           {url: 'http://www.jo-b.com'}
       ]
   };
   $scope.remove = function(index) {
       $scope.user.websites.splice(index, 1);
   };
   $scope.add = function() {
       $scope.user.websites.push({ url: ''});
   };
});
```

Pruébalo en: http://bit.ly/XHLEWQ

En la plantilla, tenemos una directiva ngRepeat que itera sobre los sitios web en el perfil del usuario. Cada directiva input en el bloque repetido, es el dato enlazado al correspondiente website.url en el modelo user.websites. La función ayudante se encarga de añadir y remover elementos hacia y desde la matriz, y el enlace de datos AngularJS hace el resto.

> `NOTA` Resulta tentador que para cada elemento website en la matriz websites esta sea una cadena simple que contenga la URL. Esto no funcionará ya que, en JavaScript, las cadenas son analizadas por valor y entonces la referencia entre la cadena en el bloque ngRepear y la cadena en la matriz se perderá cuando modifiques el valor del input.      

### Validando inputs repetidos.

El problema con este enfoque viene cuando quieres hacer el trabajo de validación en estos campos repetidos. Necesitamos que cada input tenga un único nombre dentro del formulario con el fin de acceder a la validación de ese campo, $valid, $invalid, $pristine, $dirty, etc. Desafortunadamente, AngularJS no te permite generar dinámicamente el atributo nombre para la directivas input. El nombre debe ser una cadena fija.

Resolvemos este problema usando formularios anidados. Cada uno se expone en el ámbito actual, entonces si colocamos un formulario anidado dentro de cada bloque repetido que contenga las directivas input repetidas, tendremos acceso en el ámbito a la validación de campos.

Plantilla:

```
 <form novalidate ng-controller="MainCtrl" name="userForm">
     <label>Websites</label>
     <div ng-show="userForm.$invalid">The User Form is invalid.</div>
     <div ng-repeat="website in user.websites" ng-form="websiteForm">
         <input type="url" name="website"
                ng-model="website.url" required>
         <button ng-click="remove($index)">X</button>
         <span ng-show="showError(websiteForm.website, 'url')">
           Pleae must enter a valid url</span>
         <span ng-show="showError(websiteForm.website, 'required')">
           This field is required</span>
     </div>
     <button ng-click="addWebsite()">Add Website</button>
 </form>
```

Controlador:

```
app.controller('MainCtrl', function($scope) {
   $scope.showError = function(ngModelController, error) {
       return ngModelController.$error[error];
   };
   $scope.user = {
       websites: [
           {url: 'http://www.bloggs.com'},
           {url: 'http://www.jo-b.com'}
       ]
   };
});
```

Pruébalo en: http://bit.ly/14i1sTp

Aquí estamos aplicado la directiva ngForm al div, para crear un formulario anidado, que es repetido para cada website en la matriz de websites en el ámbito. Cada uno de los formularios anidados son llamados websiteForm y cada input en el formulario es llamado website. Esto significa que estamos en condiciones de acceder a la validez del ngModel para cada website desde dentro del ámbito ngRepeat.

Hacemos uso de esto para mostrar un mensaje de error cuando el campo es inválido. La dos directivas ng-show mostraran su mensaje de error cuando la función retorna true. La función showError comprueba el pasado en ngModelController para ver si este tiene la entrada pertinente para la validación en el campo $error. Podemos pasar websiteForm.website a esta función ya que este se refiere a el objeto ngModelController para nuestro campo input website.

Fuera de ngForm no podemos referencia al objeto websiteForm ( ngFormController ) en el ámbito o el objeto websiteForm.website ( ngModelController ) ya que no existen en este ámbito. Podemos, sin embargo, acceder al objeto que contiene userForm (ngFormController). Esta validación de formularios esta basada en la validación de todos sus inputs hijos y formularios. Si uno de los websiteForms es inválido, entonces lo es userForm. El div en la parte superior del formulario despliega un mensaje de error global sólo si userForms.$valid es verdadero.   

## Manejar el envío de formularios HTML tradicionales

En esta sección echamos un vistazo a cómo AngularJS maneja el envio de formularios. Aplicaciones AJAX de una sola página, para lo cual AngularJS es perfecto, no tiende a seguir el mismo proceso de envío directo al servidor como lo hace las aplicaciones web tradicionales.  Pero alguna veces su aplicación debe soportar esto. Aquí mostramos los diferentes escenarios de envío que puede que desee implementar cuando enviamos datos del formulario al servidor.

### Enviando formularios directamente al el servidor

Si usted incluye un atributo action en un formulario en una aplicación AngularJS, entonces el formulario se enviará de forma normal a la URL definida en la acción:  

```
<form method="get" action="http://www.google.com/search">
  <input name="q">
</form>
```

Pruébalo en: http://bit.ly/115cQgq

> `NOTA` Tenga en cuenta que la vista previa en Plnkr bloqueará la redirección a Google.

### Manejando eventos de envio de formulario

Si no incluyes el atributo action, entonces AngularJS asume que vamos a administrar los envíos de formulario en el lado del cliente llamando la función en el ámbito. En este caso, AngularJS impedirá que el formulario intente directamente enviar al servidor.

Podemos desencadenar esta función del lado del cliente usando la directiva ngClick en un botón (button) o la directiva ngSubmit en el formulario (form).  

> `NOTA` Usted no debe usar ambas directivas ngSubmit y ngClick en un mismo formulario porque el navegador desencadenará ambas directivas y usted obtendrá doble envio.

### Usando ngSubmit para manejar el envío del formulario

Para usar ngSubmit en un formulario, usted proporciona una expresión que será evaluada cuando el formulario es enviado. El envío de formulario pasará cuando el usuario presione Enter en uno de los campos o haga click en uno de los botones:

```
<form ng-submit="showAlert(q)">
  <input ng-model="q">
</form>
```

Pruébalo en: http://bit.ly/ZQBLYj

Aqui, presionamos Enter mientras en el input llamará el método showAlert.

## Restableciendo el formulario de información del usuario

En nuestro formulario de información de usuario, no gustaría cancelar los cambios y restablecer el formulario a su estado original. Hacemos esto manteniendo una copia del modelo original con el cual podemos reescribir cualquier cambio que el usuario ha hecho.     

Plantilla:

```
 <form name="userInfoForm">
     ...
     <button ng-click="revert()" ng-disabled="!canRevert()">Revert
         Changes</button>
 </form>
```

Controlador:

```
app.controller('MainCtrl', function($scope) {
 //...
 $scope.user = {
   //...
 };
 $scope.passwordRepeat = $scope.user.password;
 var original = angular.copy($scope.user);
 $scope.revert = function() {
   $scope.user = angular.copy(original);
   $scope.passwordRepeat = $scope.user.password;
   $scope.userInfoForm.$setPristine();
 };
 $scope.canRevert = function() {
   return !angular.equals($scope.user, original);
 };
 $scope.canSave = function() {
   return $scope.userInfoForm.$valid &&
       !angular.equals($scope.user, original);
 };
});
```

Pruébalo en: http://bit.ly/17vHLWX

Aquí tenemos un botón para revertir el modelo a su estado original. Haciendo click en esta boton, llama a la función revert() en el ámbito. El botón se desactiva si canRevert() retorna false.

En el controlador, puede ver que usamos angular.copy() para hacer una copia del modelo y colocarlo en una variable local. El método revert() copia este original sobre el modelo user funcional y establece el formulario de regreso a su estado original de manera que todas las clases CSS no son más establecidas a ng-dirty.

## Resumen

En este capítulo hemos visto como AngularJS extiende los controles de formularios HTML estándar para proporcionar una más flexible y poderoso sistema para obtener una entrada del usuario. Se permite una separación del modelo de la vista a través de ngModel y proporciona mecanismos para seguir cambios y validaciones de valores de inputs a través de las directivas de validación en el objeto ngFormController.

En el proximo capitulo veremos como administrar de la mejor forma la navegación alrededor de nuestra aplicación. Veremos como AngularJS soporta deep linking para mapear URLs directamente a aspectos de nuestra aplicación y como usar ngView automáticamente para desplegar contenido relevante al usuario basado en la actual URL.


