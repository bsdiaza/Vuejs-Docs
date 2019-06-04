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

```
```
```
```
```
```
```
```
```
```
```
```
