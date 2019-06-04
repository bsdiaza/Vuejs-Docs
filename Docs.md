# Introduccion
Vue es un framework progresivo para el dearrollo de interfacez de usuario. Este freamwork se encuantra enfocado unicamente en la capa de visualizacion de un proyecto, ademas de ser facil de tomar e integrar a cualquier proyecto. Por otro lado Vue tambien es capaz de enpoderar aplicacioes Single-page en combinacion  de mas herramientas.

## Instalacion
La manera mas sencilla de usar Vuejs es agregando la libreria de Vuejs en el archivo `index.html`
```html
<!--Version de desarrollo-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
ó
```html
<!--Version de produccion-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```
## Rederizacion declarativa
El nuecleo de Vue.js es un sistema que permite la renderizacion declarativa de la informacion en el DOM usando la sintaxis sencilla
###HTML
```html
<div id="app">
  {{ mmensaje }}
</div>
### JS
```
```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```
Con la ejecucion de estos archivos se logra crear una aplicacion sencilla de Vue.js done la informacion mostrada en el DOM 
se encuentra vinculada con la ejecucion de nuestra aplicacion en Vue, obteniendo la renderizacion de nuestro mensaje en el DOM
```
Hello Vue!
```
Ademas de la renderizacion de la informacion almacenada se pude vincular atributos
```html
<div id="app-2">
  <span v-bind:title="message">
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
  </span>
</div>
```
```javascript
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: 'You loaded this page on ' + new Date().toLocaleString()
  }
})
```
El comando `v-bind` utilizado para vincular la variable `message` es una directiva de Vue.js. Todas las directivas
proporcionadas por este framework tienen el prefijo `v-`son usadas para proveer un comportamiento reactivo al momento
de la renderizacion del DOM. En este caso particular significa que este elemento titulo se encuentra sincornizado con la
propiedad mensaje en la instancia de Vue.

## Directivas condicionales y ciclos
La manera de renderizar de manera condicional de etiquetas en el DOM, es por medio de la directiva `v-if`.
```html
<div id="app-3">
  <span v-if="seen">Now you see me</span>
</div>
```

```javascript
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```
Esta directiva se comporta de manera tal que todo lo que se encuentre dentro de la etiqueta que la declare, incluyendo 
la etiqueta en si, se reenderice solo si la propiedad vinculada a esta directiva existe, y en caso de ser de tipo `bool`
debe ser `true`. 

Para el manejo de ciclos dentro de la aplicacion existe la directiva `v-for`, esta directiva permite iterar etiquetas o
sectores completos de la aplicacion basandose en propiedades iterables de la aplicacion.
```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
```javascript
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```
DOM
```
1. Learn JavaScript
2. Learn Vue
3. Build something awesome 
```
## Entradas de usuario 
Para permitir a alos usuarios de la aplicacion interactuar con ella existe la directiva `v-on`, la cual permite
adjuntar receptores de eventos a que invoquen metodos definidos dentro de la instancia Vue.

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```

```javascript
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```
La ejecuacion de esta instancia Vue renderizara el mensaje especificado, y un boton que al hacer activar el evento click,
ejecutara la funcion reverseMessage. Cabe resaltar que en esta caso se observa el comportamiento reactivo de Vue, ya que 
no hace falta alterar el DOM para actualizar el mensaje.

Para la vinculacion bidireccional de Vue.js provee la directiva v-model que permite vincular las entradas de usuario, al
DOM renderizado.
```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

``` javascript
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```
Esta instancia de Vue producira como resultado un DOM en el cual tanto el el texto dentro de la etiqueta `<p></p>`, como
el texto ingresado a traves del input se encuentran sincronizados en tiempo real.

## Desarrollando con componentes
El sistema basado en componentes es otro concepto importabte de Vue, debido a que este metodo de abstraccion permite construir grandes y escalables aplicaciones compuestas de pequeños, modulares y reutilizables componentes. 

![picture](assets/components-structure.png)

En Vue, un componente es en escencia una instancia de Vue con opciones predefinidas. Para registrar un componente se debe declarar sus propiedades y su identificador dentro de Vue.

```javascript
Vue.component('todo-item', {
  template: '<li>This is a todo</li>'
})
```
Luego este componente puede ser utilizado dentro de otro por medio de su identifiacdor
```html
<ol>
  <!-- Crear una instancia del componente todo-item -->
  <todo-item></todo-item>
</ol>
```
pero esto renderizaria el mismo texto para toda instancia del componente todo-item. Haciendo uso de las directivas de 
Vue este componente puede ser aprovechado de mejor manera, alterando el componente para que poseaa un parametro.
```javascript
Vue.component('todo-item', {
  // The todo-item component now accepts a
  // "prop", which is like a custom attribute.
  // This prop is called todo.
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})
```
Con este nuevo parametro podemos hacer uso de la directiva `v-bind` y la drectiva `v-for` para iterar sobre un arrelo de objetos.
```html
div id="app-7">
  <ol>
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
</div>
```
```javascript
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
DOM
```
1. Vegetables
2. Cheese
3. Whatever else humans supose to eat
```
Aunque este ejemplo es ideal debido al uso del componente a traves de las directivas, en aplicaciones de mayor tamaño es necesario dividir la aplicacion completa en tres componenetes, con el fin de hacerla mas maejable
```html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```
# Instancia Vue
## Creando la instancia
Toda aplicacion en Vue es inicializada crando una nueva instacia de Vue con la funcion
```javascript
var vm = new Vue({
  // opciones
})
```
Cuando una instancia de Vue es creada debe ser atribuido el objeto que contiene las opciones con las cuales sera creada. Una aplicacion en Vue consiste en la raiz de una instancia Vue, opcionalmente organizada en un arbol encadenado de componentes reutilizables. Este arbol puede lucir de esta manera.  
```
Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```
## Informacion y metodos
Cuando una instancia Vue es creada, esta agrega todas las propiedades encontradas en el objeto `data` a sistema reactivo de Vue. Cuando los valores de estas propiedades cambian, la vista reaccionara actualizando para emperejar los nuevos valores.
```javascript
// Objeto data
var data = { a: 1 }

// Se agrega el objeto data a la instancia Vue
var vm = new Vue({
  data: data
})

// Obteniendo la propiedad desde la instancia Vue
// Retorna el valor del objeto data original
vm.a == data.a // => true

// Estableciendo la propiedad en la instancia
// Tambien afecta el objeto original
vm.a = 2
data.a // => 2

// y viceversa
data.a = 3
vm.a // => 3
```
Cuando los atributos cambian, la vista volvera a renderizarce demostrando que las propiedades en el objeto data son unicamente reactivos, pero si estos atributos existan cuando la instancia fue creada. Si un nuevo atributo es declarado despues de la creacion de que la instancia Vue es creada, la aplicacion no "reaccionara" a los cambios de dicho atributo. 

La unica manera de que la aplicacion no reaccione a los cambios realizados al objeto data con el que se inicializo la propiedad data de la instancia, es haciendo uso de la funcion `Object.freeze()`. Esta funcion previene las propiedades existentes de un objeto sean alteradas posteriormente.

```javascript
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
```html
<div id="app">
  <p>{{ foo }}</p>
  <!-- Este metodo no actualizare la variable -->
  <button v-on:click="foo = 'baz'">Change it</button>
</div>
```
Ademas de las propiedades incluidas en el objeto data, cada instancia Vue expone varias propiedades y metodos por defecto. Estos tienen el prefijo $ para diferenciarlos de los que son definidos al momento del desarrollo.
```javascript
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch is an instance method
vm.$watch('a', function (newValue, oldValue) {
  // This callback will be called when `vm.a` changes
})
```
## Ciclos de vida de instancias
Cada instancia creada pasa por una serie de pasos de inicializacion cuando es creada, como preparar la informacion observable, compilar la plantilla, montar la instancia en el DOM y actualizarlo cuando la informacion vinculada cambie. En cada uno de estos pasos se ejecutan funciones del ciclo de vida de la instancia, permitiendo ejecutiar codigo propio en etapas especificas. Como lo puede ser la funcion `created`.
```javascript
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` hace referencia a la vista-modelo de la instancia
    console.log('a is: ' + this.a)
  }
})
```
Existen las etapas del ciclo de vida de una instancias son: `beforeCreate, created, beforeMount, mounted, beforeUpdate, updated, activated, deactivated, beforeDestroy, destroyed, errorCaptured`.
Las cuales se pueden ver en este diagrama.
![picture](assets/lifecycle-diagram.png)

# Sintaxis de plantilla
Vue.js hace uso de una plantilla basada en la sintaxis HTML que permite vincular declarativamente el DOM renderizado con la instancia Vue corriendo en una capa inferior. Vue compila las plantillas a a funciondes de renderizado de un DOM virtual. Combinando esto con la reactividad del sistema, Vue es capaz de determinar la minima cantidad de componentes que deben ser renderizados de nuevo y aplicar la minima cantidad de cambios al DOM cuando ocurra un cambio de estado.

## Interpolaciones
### Texto
La manera mas basica de vincular informacion es la interpolacion de texto usando "corchetes"
```html
<span>Message: {{ msg }}</span>
```
La etiqueta entre corchetes sera remplazada con el valor de la propiedad `msg` en el correspondiente objeto data, esta etiqueta sera tambien actualizada cuando la propiedad msg cambie. Tambien pueden ser realizadas interpolaciones que no se actualicen usando la directiva `v-once`, pero esto tambien afectara otras vinculaciones en el mismo nodo.

```html
<span v-once>This will never change: {{ msg }}</span>
```
### HTML directo
los corchetes dobles interpretan la informacion como texto plano, no como HTML, para exponelo como HTML real, es necesario hacer uso de la directiva `v-html`

```html
<p>Usando corechetes: {{ rawHtml }}</p>
<p>Usando directiva v-html: <span v-html="rawHtml"></span></p>
```
```
Usando corchetes: <span style="color: black"> This should be black</span>
Usando directiva v-html: This should be black
```
### Atributos
Los corchetes dobles no pueden ser utilizados en atributos propios de HTML: En vez, se debe usar la directiva `v-bind`:
```html
<div v-bind:id="dynamicId"></div>
```
En el caso de los atributos booleanos, donde solo su existencia implica el valor `true`.

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```
Si `isButtonDisabled` tiene el valor de nulo, indefinido o false, el atributo `disabled` no sera incluido in el boton renderizado.Para ello se recomienda usar `v-bind:disabled="!!isButtonDisabled""`, aunque esto se eplicara mas adelante.

### Usando expresiones javascript
Vue.js soporta el uso de expresiones en javascript dentro de sus vinculaciones.

```javacript
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}
```
```html
<div v-bind:id="'list-' + id"></div>
```
Estas expresiones seran evaluadas como codigo en Javascript en el enfoque de la instancia Vue. Pero la vinculacion solo puede tener una simple expresion, asi que estos ejemplos no servirian:

```html
<!-- Esto es una declaracion, no una expresion -->
{{ var a = 1 }}

<!-- El control de flujo no funciona -->
{{ if (ok) { return message } }}
```
## Directivas
Las directivas son atributos especiales declarados con el prefijo `v-`. El valor de un atributo de una directiva se espera que sea una expresion simple en Javascript y su trabajo es reaccionar activamente aplicando los efectos al DOM cuando el valor de su expresion cambie. Como ya lo vimos con la directiva `v-if`
```html
<p v-if="seen">Now you see me</p>
```
### Argumentos
Algunas directivas pueden tomar un argumento denotado por dos puntos despues del nombre de la directiva.

```html
<!-- vincula el atributo href a la propiedad url -->
<a v-bind:href="url"> ... </a>
```
```html
<!-- vincula el evento click a la funcion doSomething -->
<a v-on:click="doSomething"> ... </a>
```
### Argumentos dinamicos
El nombre del atributo a vincular tambien puede ser llamado de manera dinamica como una expresion de javascript, agregando los corchetes cuadrados al rededor del nombre del atributo, 

```html
<a v-bind:[attributeName]="url"> ... </a>
```

```html
<a v-on:[eventName]="doSomething"> ... </a>
```
Si la propiedad attributeName contuviera el valor href y el la propiedad eventName cuentuviera el valor click, estas expresiones equivaldrian a 
```html
<a v-bind:href="url"> ... </a>
```

```html
<a v-on:click="doSomething"> ... </a>
```
Se espera que los argumentos dinamicos sean te tipo `string`, con la excepcion del valor `null`, donde el valor `null` se puede tomar como una desvinculacion de dicho atributo. Ademas estas expresiones tienen ciertas restricciones de sintaxis debido a que ciertos caracteres o son permitidos dentro de un atributo HTML como espacios o comillas.

### Modificadores
Modificadores son postfijos especiales denotados por un punto, el cual indica que la directiva debe ser vinculada de manera especial. Por ejemplo el modifiacdor `.prevent` le indica a la directiva `v-on` que llame la funcion `event.preventDefault()` al momento de accionar el evento.
```html
<form v-on:submit.prevent="onSubmit"> ... </form>
```
## Abreviaciones
El prefijo `v-` sirve para identificar las directivas de Vue.js, pero estas directivas pueden ser abreviadas.
### Abreviacion `v-bind`
```html
<!-- sintaxis completa -->
<a v-bind:href="url"> ... </a>

<!-- abreviacion -->
<a :href="url"> ... </a>

<!-- abreviacion con argumentos dinamicos -->
<a :[key]="url"> ... </a>
```
### Abreviacion `v-on`
```html
<!-- sintaxis completa -->
<a v-on:click="doSomething"> ... </a>

<!-- abreviacion -->
<a @click="doSomething"> ... </a>

<!-- abreviacion con argumentos dinamicos -->
<a @[event]="doSomething"> ... </a>
```
# Propiedades calculadas y observadores
## Propiedades calculadas
Las expresiones internas de las plantillas son muy convenientes, pero, pero estan pensadas para operaciones simples. Agregar demaciada logica a las plantillas puede hacerlas insostenibles e ineficientes. Como:
```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
En este punto la plantilla deja de ser simple y declarativa, y esto puede empeorar si el mensaje revertido es incluido mas de una vez. Para este problema se debe usar la propiedad computed de Vue.

### Ejemplo basico
```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```
DOM
``` 
Original message: "Hello"
Computed reversed message: "olleH"
```
En este ejemplo se a declarado las propiedad calculada `reverseMessage`. La funcion que se preovea sera usada como una funcion `getter` para la propiedad `vm.reversedMessage`
```javascript
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```
Estas propiedades calculadas pueden ser vinculadas con la directiva v-bind a la plantilla, como una propiedad normal. La instancia Vue es conciente que `vm.reversedMessage` depende de `vm.message`, asi que actualizara cualquier vincualcion realizada a la propiedad `vm.reversedMessage` cuando `vm.message` sea alterado. Pero sin las repercuciones de ejecucion de esta funcion cada vez que se necesite.

### Almacenamiento en cache vs Metodos
El resultado obtenido anterior mente con propiedades calculadas tambien puede ser obtenido invocando un metodo dentro de una expression en la plantilla de esta manera:
```html
<p>Reversed message: "{{ reverseMessage() }}"</p>
```
```javascript
// en el componente
methods: {
  reverseMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
En vez de propiedades computadas, puede ser definida la misma funcion como su fuera un metodo. Pero apesar de que el resultado final es el mismo, la principal diferencia en entre estas dos aproximaciones de la solucion es que la propiedad calculada es guardada en cache basada en las reacciones de sus dependencias. La propiedad calculada solo sera re-evaluada cuando alguna de sus dependencias reactivas ha cambiado. En comparacion con la aproximacion por medio de metodos, la cual nunca es vuelta a renderizar.

### Propiedades calculadas vs observadas
Vue provee una manera mas generica de observar y reaccionar a los cambios de informacion en una instancia Vue: `Observadores de propiedades`. Cuando se tiene alguna propiedad que necesite cambiar basada en otra, suenta tentador hacer un sobre uso de la propiedad `watch`.
```html
<!-- watch -->
<div id="demo">{{ fullName }}</div>
```
```javascript
<!-- watch -->
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```
La aproximacion usando la propiedad watch es imperativa y repetitiva comparada con la version calculada.
```javascript
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```
### Setter calculado
Las propiedades calculadas son por defecto unicamente `getter`, pero tambien puede ser usada como `setter`.

```javascript
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```
De esta manera cuando se llame ejecute `vm.fullName = 'John Doe'`, el `setter` sera invocado y las propiedades `vm.firstName` y `vm.lastName` seran actualizadas respectivamente.

## Observadores
Mientras que as propiedades calculadas son mas apropiedas en la mayoria de los casos, hay momentos en los que un observador personalizado es necesario. Debido a esto es que Vue provee otras los observadores, estos son utiles cuando se llevan a cabo operaciones asincornas y costosas en respuesta a informacion cambiante.

```html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```
```html
<!-- Since there is already a rich ecosystem of ajax libraries    -->
<!-- and collections of general-purpose utility methods, Vue core -->
<!-- is able to remain small by not reinventing them. This also   -->
<!-- gives you the freedom to use what you're familiar with.      -->
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // Siempre que la pregunta cambie esta funcion sera ejecutada
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // Inicializamos la funcion a ejecutar de cuando el usuario termine de 
    // escribir su pregunta
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
  }
})
</script>
```
En este caso, haciendo uso de la opcion de observadores permitira llevar a cabo una operacion asincrona de acceso a la API, limitar que tan frecuentemente ser realizara dicha operacion y establecer estados intermedios hasta que finalmente se obtenga la respuesta final.

# Clases y vinculacion de estilos
#### Modificadores de eventos
Es muy usual necesitar hacer uso de `event.preventDefault()` o de `event.stopPropagation()` dentro de los elementos que lo soportan, para esto Vue provee modificadores de eventos para la directiva `v-on`.
* .stop
* .prevent
* .capture
* .self
* .once
* .passive
```html
<!-- La propagacion del evento click sera detenida -->
<a v-on:click.stop="doThis"></a>

<!-- El evento submit no recargara la pagina -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- Los modificadores pueden ser encadenados -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- Solo la declaracion del modificador -->
<form v-on:submit.prevent></form>

<!-- uso de capture cuando se desea atrapar un evento -->
<!-- ej. la activacion de un elemento interno se hace primero aqui antes que en ese elemento -->
<div v-on:click.capture="doThis">...</div>

<!-- solo se activa el evento si el objetivo es el elemnto en si -->
<!-- ej. no de un elemento hijo -->
<div v-on:click.self="doThat">...</div>
```
