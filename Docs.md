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
Una necesidad comun de vinculacion de datos es manipular la lista de clases de un elemento y sus estilos asociados. Dado que ambos son atributos, podemos usar v-bind para establecerlos, solo hace falta calcular el string final con las expresiones. Pero manejar la concatenacion es suceptible a errores, debido a esto cuando `v-bind` es usado con los atributos `class` y `style`. Permitiendo evaluar ademas de strings, objetos o arreglos.

## Vinculacion de clases HTML
### Sintaxis de objetos 
Un objeto puede ser usado con la directiva `v-bind:class` para cargar dinamicamente clases:
```html
div v-bind:class="{ active: isActive }"></div>
```
Esta sintaxis representa la presencia de la clase `active` sera determinada por la veracidad del valor de la propiedad `isActived`.

Se puede tener multiples clases activas adjuntas teniendo mas campos en el objeto. La vinculacion `v-bind:class` puede coexistir con el atributo plano `class`

```html
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```
y las propiedades siguientes
```javascript
data: {
  isActive: true,
  hasError: false
}
```
tendra como resutado el elemento con las clases
```html
<div class="static active"></div>
```
Cuando las propiedades `isActived` o `hasError` sean alteradas, la lista de clases sera actualizada. 

El objeto realcionado no debe ser declarado en el elemento, puede ser declarado como propiedad de la instancia.
```html
<div v-bind:class="classObject"></div>
```
```javascript
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
a su vez el atributo `class` puede ser declarado con una propiedad calculada.
```html
<div v-bind:class="classObject"></div>
```
```javascript
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
### Sintaxis de arreglo
Un arreglo puede proveerse con `v-bind:class` para aplicar una lista de clases
```html
<div v-bind:class="[activeClass, errorClass]"></div>
```
```javascript
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
Tambien se puede adjuntar una clase en una lista condicional con una expresion ternaria
```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
### Con componentes
Cuando se use el atributo `clase` en un componente personalizado, esas clases seran agregadas al elemento raiz del componente. Pero las clases existentes de este elemento no seran sobre escritas.
```javascript
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```
Luego se pueden agregar alguas clases cuando este componente sea usado
```html
<my-component class="baz boo"></my-component>
```
El HTML renderizado sera
```html
<p class="foo bar baz boo">Hi</p>
```
## Vinculacion de estilos
### Sintaxis de objeto
La vinculacion de estilos usando `v-bind:style` es bastante parecida a CSS nativo, por medio de un objeto.
```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
```javascript
data: {
  activeColor: 'red',
  fontSize: 30
}
```
Aunque es como mejor practica es mejor vincular el objeto directamente
```html
<div v-bind:style="styleObject"></div>
```
```javascript
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
### Sintaxis de arreglo
La sintaxis de arreglo para `v-bind:style` permite aplicar multiples objetos de estilo en el mismo elemento
```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
### Auto prefijo
Cuando se usa una propiedad CSS que requiera prefijos en el atributo `v-bind:style`, como `transform`, VUe detectara automaticamente estas propiedades y agregara los prefijos apropiados a los estilos aplicados.

### Valores multiples
Puede preveerse arroglos de multiples prefijos para una propiedad del estilo
```html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
Esto solo renderizara el ultimo valor en el arreglo que el buscador soporte.

# Renderizacion condicional
## `v-if`
La directiva `v-if` es usada para renderizar de manera condicional un bloque HTML. El bloque sera renderizado si la expresion retorna un valor verdadero.
### Grupos condicionales con `v-if` en `<template>`
Debido a que `v-if` es una directiva, debe ser agregado a un elemento unitario. Pero si se desea reenderizar de manera condiconal mas de un elemento en este caso se puede utilizar `<template>` como un agrupador de elementos invisible.
```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```
### `v-else`
Puede ser usada la directiva `v-else` para  indicar una condicion encadenada `else` para la directiva inicial `v-if`
```html
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>
```
El elemento que hara uso de la directiva `v-else` debe seguir inmediatamente al elemento que hace uso de la directiva `v-if`
### `v-else-if`
Asi mismo la directiva `v-else-if` como un condicional encadenado.
```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```
Asi como la directiva `v-else`, la directiva `v-else-if` debe ir inmediatamente despues del elemento que hace uso de la directiva `v-if`

### Controlando elementos reutilizables
Vue intentara renderizar los elementos de la manera mas eficiente posible, frecuentemente re-usandolos en vez de renderizarlos de nuevo. 

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```
En este caso el elemento `<input>` sera detectado como reutilizable y no sera renderizado de nuevo, solo se cambiara su atributo `placeholder` manteniendo el valor alamacenado dentro de este `<input>`, al momento cambiar la propiedad `loginType` entre los dos valores establecidos.

Aunque debido a los requerimientos de la aplicacion este comportamiento puede no ser siempre deseable, en estos casos existe la posibilidad de aclarar que estos elementos son completamente diferentes haciendo uso de la directiva `key`.
```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
Ahora estos elementos seran renderizados como elemntos nuevos al momento de cambiar la propiedad `loginType`.

## `v-show`
Otra opcion para renderizar de manea condicional un elemento es la directiva `v-show`. EL uso y resultado es casi el mismo.
```html
<h1 v-show="ok">Hello!</h1>
```
La principal diferencia es que el elemento seguira renderizado y permanecera en el DOM, pero la propiedad `display` del estilo es actualizada para que el elemento no sea visualizado.

## `v-if`vs`v-show`
`v-if` destruye y crea los elementos relacionados al momento del cambio, ademas de ser ejecuatado de manera perezosa, renderizando los elementos solo hasta que el valor de la  expresion sea verdadera, teniendo un mayor costo al momento del cambio del valor de la expresion.
`v-show` por otro lado solo "esconde" los elementos segun el valor de la expresion, teniendo un mayor costo de renderizacion inicial ya que los elementos son igualmente renderizados.
## `v-if` con `v-for`
Aunque no es recomendable hacer uso de las directivas `v-if` y `v-for` juntas. En caso de ser usadas de esta manera, la directiva `v-for` tiene  una mayor precedencia que la directiva `v-if` al momento de ser evaluadas.

# Renderizando listas
## Mapear arreglos a elementos con `v-for`
La directiva `v-for` puede ser usada para renderizar una lista de items basada en un arreglo. La sintaxis para hacer uso de esta directiva con un arreglo de items es `item in items`, donde item es el alias del elemento iterble del arreglo.
```html
<ol id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ol>
```
```javascript
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
Resultado
```
1. Foo
2. Bar
```
Dentro de los bloques se tiene completo acceso a las propiedades de las propiedades del padre. `v-for` ademas soporta un segundo argumento opcionarl para representar la posicion del elemento dentro de la lista.
```html
<ol id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ol>
```

```javascript
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
Resultado
```
1. Parent - 0 - Foo
2. Parent - 1 - Bar
```
## `v-for` con objetos
Tambien puede ser usada la directiva `v-for` para iterar a traves de propiedades de un objeto. 
```html
<ol id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ol>
```
```javascript
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```


```
1. How to do lists in Vue
2. Jane Doe
3. 2016-04-10
```
Tambien puede usarse un segundo argumento para el nombre de la propiedad del objeto, y un tercero para la posicion del objeto dentro de la propiedad.
```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```
```
1. title: How to do lists in Vue
2. author: Jane Doe
3. publishedAt: 2016-04-10
```
## Manteniendo estado
Cuando Vue se encuentra actualizando una lista de elementos renderizados con la directiva `v-for`, por defecto actualizara cada posicion de la lista con la informacion del elemento que ahora ocupa dicha posicion. Este modo por defecto es eficiente pero solo es apropiado cuando cando la visualizacion de la inforamcion no recae en componentes internos. 

Para darle a Vue una pista para que pueda hacer un seguimiento mas apropiado a cada item de la lista, se sugiere usar el atributo `key`, para que Vue pueda reusar y reorganizar los elementos de la lista.
```html
<div v-for="item in items" v-bind:key="item.id">
  <!-- content -->
</div>
```
## Deteccion de cambios en arreglos

### Metodos de mutacion
Vue contiene un grupo de metodos de mutacion para arrelos que tambien activaran la actualizacion de las vistas.
* `push()`
* `pop()`
* `shift()`
* `unshift()`
* `splice()`
* `sort()`
* `reverse()`

### Remplazando un arreglo

Existen metodos que no mutan el arreglo original pero si retornan un nuevo arreglo, como lo son `filter()`, `concat()` y `slice()`. Cuando se utilicen este tipo de metodos se puede remplazar el arreglo antguo con el nuevo.
```javscript
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```
### Advertencias
Debido a limitaciones en javascript, Vue no puede detectar algunas mutaciones, pero existen alternativas que lo permiten.
```javascript
// Arreglos
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // NO reactivo
vm.items.length = 2 // NO reactivo
Vue.set(vm.items, indexOfItem, newValue) // Reactivo
vm.items.splice(indexOfItem, 1, newValue) // Reactivo
```
```javascript
// Objetos
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` es reactivo
vm.b = 2
// `vm.b` NO es reactivo
// Vue.set(vm, 'b', 2) Reactivo
```
### Visualizando resultados filtrados u organizados
Para visualizar un arreglo filtrado u organizado pero sin actualmente alterar el arreglo original se puede hacer uso de una propiedad calculada que retorne el arreglo de la manera deseada.
```html
<li v-for="n in evenNumbers">{{ n }}</li>
```
```javascript
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
En casos en los que una propiedad calculada no sea factible, puede ser usado un metodo que cumpla la misma funcion.
```html
<li v-for="n in even(numbers)">{{ n }}</li>
``` 
```javascript
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
### `v-for` con rango
La directiva `v-for` puede tomar tambien un entero como parametro. En este caso repetira la plantilla ese numero de veces.
```html 
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```
Resultado
```
1 2 3 4 5 6 7 8 9 10
```
### `v-for` con un componente

La directiva `v-for` puede ser directamente usada sobre un componente.
```html
<my-component v-for="item in items" :key="item.id"></my-component>
```
de cualquier manera esto no proporcionara los parametros directamente al componente. Para hacer eso estos parametros deben ser proporcionados de manera especifica.
```html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```
# Soporte de eventos
## Escuchando eventos
La directiva `v-on` puede ser usada oara escuchar los eventos del DOM y ejecutar funciones cuando estos eventos son accionados.
```html
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
```
```javascript
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```
## Mentodos manejadores de eventos
La logica para muchos eventos no es sencilla y mantener esa funcionalidad en la expresion de la directiva `v-on` no es factible. En este caso VUe permite hacer llamado de metodos que realicen dicah funcionalidad.
```html
<div id="example-2">
  <button v-on:click="greet">Greet</button>
</div>
```
```javscript
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  //los metodos deben ser difinidos dentro del objeto metodo
  methods: {
    greet: function (event) {
      // `this` dentro de los metodos apunta a la instancia Vue
      alert('Hello ' + this.name + '!')
      // `event` es nativo de DOM
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})
```
## Metodos manejados en linea
En vez de vincular directamente el nombre de un metodo, tambien estos metodos pueden ser usados como en ejecucion de codigo javascript.
```html
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```
```javascript
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```
Para acceder al evento original del DOM en un estado manejado. El evento puede ser proporcionado como para metro con `$event`.
```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```
```javscript
methods: {
  warn: function (message, event) {
    // ahora se tiene acceso al evento nativo
    if (event) event.preventDefault()
    alert(message)
  }
}
```
## Modificadores de eventos
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
## Modificadores de teclas
Para escuchar eventos de teclas especificas Vue permite añadir modificadores de teclas para `v-on`.
```html
<!-- solo ejecuta `vm.submit()` cuando la `tecla` es `Enter` -->
<input v-on:keyup.enter="submit">
```
# Vinculacion de formularios
La directiva `v-model` puede ser usada para crear una vinculacion de informacion bidireccional en cualquier elemento de entrada html.
## Texto
```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```
Este codigo vinculara en tiempo realla propiedad `value` de el elemento `<input>` con el texto mostrado.
## Texto multi-linea
```html
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```
Este ejemplo representara en el elemento`<p>` la inforacion ingresada en el elemento `<textarea>`, incluyendo el formato de saltos de linea.
## checkbox
```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```
Este ejemplo representara en el `<label>` el valor vinculado del checkbox
```html
<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
```
```javscript
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```
Este ejemplo vincula la propiedad checkedNames como un arreglo que contiene la propiedad `value` de todos los checkbox que se encuentren seleccionados.

## Select
```html
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>Selected: {{ selected }}</span>
```
```javascript
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```
Este ejemplo visualiza en el elemento `<span>` el valor seleccionado en la entrada `<select>`.
## Vinculacion de valores
La vinculacion de valores cuando de las entradas value puede ser personalizada.
```html
<!-- seleccion el string 'a' cuando se encuentra validado -->
<input type="radio" v-model="picked" value="a">

<!-- la variable 'toggle' es de tipo verdadero o falso -->
<input type="checkbox" v-model="toggle">

<!-- `seleccion el string 'abc' cuando la primera opcion es seleccionada -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```
Estos valores tambien pueden ser seleccionados de manera dinamica usando la directiva `v-bind`.
### checkbox 
```html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
```
```javascript
// cuando se encuentra seleccionado
vm.toggle === 'yes'
// cuando no se encuentra seleccionado
vm.toggle === 'no'
```
### radio
```html
<input type="radio" v-model="pick" v-bind:value="a">
```
```javascript
// cuando se encuentra seleccionado
vm.pick === vm.a
```
### Select Options
```html
<select v-model="selected">
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```
```javascript
// when selected:
typeof vm.selected // => 'object'
vm.selected.number // => 123
```
## Modificadores
### `.lazy`
Por defecto `v-model` sincroniza el elemento de entrada con la propiedad de Vue despues de cada evento ocurrido en la entrada. El modificador `lazy` hace que este evento se accione despues del evento `change`
```html
<!-- ssincronizado despues de "change" en vez de "input" -->
<input v-model.lazy="msg" >
```
### `.number`
Si se desea que la entrada se transforme automaticamete a tipo numero se puede usar el modificador `number`. 
```html
<input v-model.number="age" type="number">
```
### `.trim`
Si se desea eliminar los espacios en blanco se puede adicionar el modificador `trim`.
```html
<input v-model.trim="msg">
```
# Bases de los componentes
## Ejemplo base 
```javascript
// Define un nuevo componente llamado "button-counter"
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```
Los componentes son instancias con nombre de Vue reutilizables. El componente pede ser utilizado como un elemento personalizado dentro de la raiz de la instancia Vue.
```html
<div id="components-demo">
  <button-counter></button-counter>
</div>
```
```javascript
new Vue({ el: '#components-demo' })
```
Debido a que los componentes son instancias reutilizables de Vue, estos aceptan las mismas opciones `new Vue`, como `data`, `computed`, `watch`, `methods` y etapas del ciclo de vida. 

## reusando componentes
Los componentes pueden ser reutilizados las veces que se deseem debido a que Vue los transforma a elementos propios del proyecto que pueden ser llamados en cualquier momento.
```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```
Este ejemplo renderizara tres veces el componente `button-counter`. Cada vez que se renderiza el componente este lo hace como una nueva instancia, provocando que sus propiedades sean independientes de las demas instancias del mismo tipo.

### `data` debe ser una funcion
Cuando se define el componente `<button-couner>`. Para que las propiedades de la instancia sean independientes ante las demas instancias de este componente, la propiedad `data` debe ser declarada como una funcion asi cada instancia tendra una copia de la informacion retornada.
```javascript
data: function () {
  return {
    count: 0
  }
}
```
## Organizando componentes
Es frecuente organizar una aplicacion como un arbol de componentes encadenados. Para hacer uso de los componentes en las plantillas, estos deben estar registrados para que Vue los reconozca. Este registro puede ser de manera global o de manera local. 
Los componentes registrados globalmente usando `Vue.component('component-name'{..options..})` pueden ser usados en la plantilla de cuaquier instancia raiz de Vue, incluso en sus componentes internos.

## Pasando informacion a componentes internos
`Props` son atributos personalizados que pueden ser registrado en un componente. Cuando el valor es asignado a un atributo `prop`, este se vuelve una propiedad en la instancia de este componente. Para pasar un `prop` a un componente, se puede incluir este atributo en la lista de `props` de dicho componente usando la opcion `props`.
```javascript
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```
```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```
Resultado
```
My journey with Vue
Blogging with Vue
Why Vue is son fun
```
La directiva `v-for` puede ser usada para iterar sobre un arreglo u objeto del componente padre y crear un componente por cada elemento de esta lista, vinculando la informacion de dicho elemento haciendo uso de la directiva `v-bind`. 
```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```
## Elemento con raiz unica
Aunque casi todos los componentes suelen tener mas de un solo elemento interno, Vuejs no permite que exista mas de un elemento en la raiz del componente, para solucionar esto todos los componenetes deben tener todo su contenido agrupado
por un elemento padre.
```html
<div class="blog-post">
  <h3>{{ title }}</h3>
  <div v-html="content"></div>
</div>
```
Al mismo que los componentes crecen, es probable que los atributos de dicho componenten tambien aumenten, como mejor practiva de programacion, se sugiere agrupar estos atributos.
```html
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:post="post"
></blog-post>
```
```javascript
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <div v-html="post.content"></div>
    </div>
  `
})
```
## Escuachando eventos de componentes internos
Al momento de desarrollar aplicaciones suele ser necesario que los componentes internos se comuniquen de vuelta con el componente padre. Se puede definir una instancia de VUe que contenga todos las instancias del componente `blog-post` y vincular el tamaño de la fuente de todas estas instancias con la propiedad `postFontSize`.
```javscript
new Vue({
  el: '#blog-posts-events-demo',
  data: {
    posts: [/* ... */],
    postFontSize: 1
  }
})
```
```html
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
    ></blog-post>
  </div>
</div>
```
Ademas se puede agregar un boton que emita un evento para aumentar el tamaño de la fuente, haciendo uso del metodo de contruccion interno `$emit`.
```javascript
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button v-on:click="$emit('enlarge-text')">
        Enlarge text
      </button>
      <div v-html="post.content"></div>
    </div>
  `
})
```
Y hacer uso de la directiva `v-on` para ejecutar una expresion cuando el evento `enlarge-text` sea accionado.

```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += 0.1"
></blog-post>
```
### Emitienedo un valor con un evento
Para emitir un evento con un valor, el valor puede ser emitido como segundo parametro haciendo uso del mismo metodo `$emit`.
```html
button v-on:click="$emit('enlarge-text', 0.1)">
  Enlarge text
</button>
```
Este valor sera accesible al elemento padre por medio de `$event`.
```html
<blog-post
  ...
  v-on:enlarge-text="postFontSize += $event"
></blog-post>
```
o si la expresion asignada para manejar dicho evento es un metodo, este lo recebira como parametro.
```html
<blog-post
  ...
  v-on:enlarge-text="onEnlargeText"
></blog-post>
```
```javascript
methods: {
  onEnlargeText: function (enlargeAmount) {
    this.postFontSize += enlargeAmount
  }
}
```
### Usando `v-model` en componentes
Eventos personalizados tambien pueden ser usados para crear inputs personalizados que trabajen con la directiva `v-model`.
```html
<input v-model="searchText">
```
tiene la misma funcionalidad que:
```html
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>
```
Cuando la directiva `v-model` es usada en un componente, en vez de vincularlo de la siguiente manera.
```html
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```
Para que el elemento `input` se debe vincular su atributo `value` con el `prop` y al accionar el evento `input`emitir el mismo evento al padre con su valor.
```javascript
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```
```html
<custom-input v-model="searchText"></custom-input>
```
### Distribucion de contenido con parametros
Asi como elementos HTML, suele ser util ser capaz de pasar contenido a un componente.
```html
<alert-box>
  Something bad happened.
</alert-box>
```
Lo cual deberia renderizar el texto dentro del componente `alert-box`. Para hacer que esto funcione debe ser utilizada la etiqueta slot, para especificar en que parte del componente debe ser renderizado dicho componenete.
```html
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```
### Componentes dinamicos
Para renderizar dinamicamene componentes que puede ocupar un mismo espacio y que estos sean intercambiables puede ser usado el elemento Vue `<component>`, para que Vue reconozca que lo que va a ser renderizado por este elemento es un componente previamete registrado, junto con el atributo especial `is`, que sirve para especificar el identificador o el objeto que contenga la informacion del componente  que se va a renderizar en este elemento.
```html
<component v-bind:is="currentTabComponent"></component>
```
Ejempl
```html
<div id="dynamic-component-demo" class="demo">
  <button
    v-for="tab in tabs"
    v-bind:key="tab.name"
    v-bind:class="['tab-button', { active: currentTab.name === tab.name }]"
    v-on:click="currentTab = tab"
  >{{ tab.name }}</button>

  <component
    v-bind:is="currentTab.component"
    class="tab"
  ></component>
</div>
```
```javascript
var tabs = [
  {
    name: 'Home', 
    component: { 
      template: '<div>Home component</div>' 
    }
  },
  {
    name: 'Posts',
    component: {
      template: '<div>Posts component</div>'
    }
  },
  {
    name: 'Archive',
    component: {
      template: '<div>Archive component</div>',
    }
  }
]

new Vue({
  el: '#dynamic-component-demo',
  data: {
  	tabs: tabs,
    currentTab: tabs[0]
  }
})
```
### Manejando advertencias dentro de la plantilla DOM
Algunos elementos HTML como `<ul>`,`<ol>`,`<table>` y `<select>` tienen restricciones sobre que elementos pueden aparecer dentro de ellos, y ciertos elementos como `<li>`, `<tr>` y `<option>` pueden solo aparecer dentro de ciertos elementos tambien.
Esto generara conflico cuando componentes sean usados con elementos que tengan dicha restriccion.
```html
<table>
  <blog-post-row></blog-post-row>
</table>
```
El componente personalizado `<blog-post-row>` sera detectado como contenido invalido, causando errores. Para hacer uso de componetes con estos elementos restringidos se debe utilizar el atributo esecial `is`.
```html
<table>
  <tr is="blog-post-row"></tr>
</table>
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
```
```
```
```
```
```
```
