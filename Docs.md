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


```
```

```
```

```
