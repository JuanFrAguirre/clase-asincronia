# ¿Qué vamos a ver en esta clase?

En muchos casos, resulta necesario ejecutar código en un momento dado pero sin saber cuánto tomará en ejecutarse esa línea de código.

Esto puede deberse a varios motivos, código que depende de alguna condición externa a nuestro mismo código, algo que tenga que resolverse primero, un setTimeout, etc.

En estos casos, es cuando hablamos de <em>asincronía</em> en JavaScript. La <em>asincronía</em> en JavaScript implica aplicar ciertas técnicas y tener varias cosas en cuenta para resolver nuestras necesidades de manera correcta y performante.

A lo largo de esta clase vamos a conocer más acerca de:

- ¿Qué es la <strong>asincronía</strong>?

- ¿Qué son los <em>callbacks</em>?

- ¿Qué son las <em>promises</em>?

- ¿Qué es <em>async/await</em>?

##### ¿Comenzamos?

---

# <span style="color: ivory">Asincronía</span>

### Introduciéndonos a la asincronía

La asincronía es uno de los conceptos más importantes en JavaScript. Hasta ahora, siempre que escribimos código lo hemos hecho de manera **síncrona**, es decir, ejecutando nuestras tareas de manera secuencial, una tras otra. De esta manera el orden o flujo de nuestra aplicación es sencillo y fácil de observar en nuestro código

```js
funcion_uno();    // Se ejecuta primero
funcion_dos();    // Se ejecuta cuando termina funcion_uno()
funcion_tres();   // Se ejecuta cuando termina funcion_dos()
```

Sin embargo, en el mundo de la programación, tarde o temprano deberemos realizar operaciones **asíncronas**, más aún en JavaScript donde tenemos que realizar tareas que *deben esperar a que ocurra un determinado suceso* que **no** depende de nosotros, y reaccionar realizando otra tarea, pero solamente luego de que dicho suceso ocurraprogramación, tarde o temprano deberemos realizar operaciones **asíncronas**, más aún en JavaScript donde tenemos que realizar tareas que *deben esperar a que ocurra un determinado suceso* que **no** depende de nosotros, y reaccionar realizando otra tarea, pero solamente luego de que dicho suceso ocurra.

### Lenguaje no bloqueante

JavaScript es lo que se conoce como un lenguaje **no bloqueante**. Esto hace referencia a que las tareas que realizamos no se quedan *bloqueadas* esperando ser finalizadas, con lo cual se evitaría seguir con el flujo normal de ejecución de tareas.
Imaginemos que <code>funcion_dos()</code> del ejemplo anterior realiza una tarea que depende de otro factor, como por ejemplo un click del usuario. Si JavaScript fuera un **lenguaje bloqueante**, hasta que el usuario no haga ese click, JavaScript **no** seguiría ejecutando las demás funciones, sino que se quedaría *bloqueado* esperando a que se terminase esa segunda tarea.

![Asincronía en Javascript](https://lenguajejs.com/javascript/asincronia/que-es/asincronia-javascript.png)

Lo bueno, es que como JavaScript es un **lenguaje no bloqueante**, lo que hará es mover esa tarea a una lista de **tareas pendientes** a las que irá prestándole atención a medida que lo necesite, pudiendo continuar y retomar el resto de tareas que le siguen a la segunda.

### ¿Qué es la asincronía?

Pero esto no es todo. Tengamos en cuenta que pueden existir **múltiples** tareas asíncronas, estas tareas puede que terminen realizandose correctamente (*o puede que no*) y ciertas tareas pueden depender de otras, por lo que deben respetar un cierto orden. Además, es muy común que no sepamos de antemano **cuánto tiempo** va a tardar en terminar una tarea, por lo que necesitamos un mecanismo para poder controlar todos estos factores: las **promesas**, las cuales veremos algunos capítulos más adelante.

### Ejemplos de tareas asíncronas

En JavaScript no todas las tareas son asíncronas, pero hay ciertas tareas que sí lo son, veamos algunas de ellas:

- Una tarea programada con `setTimeout()`, que se ejecutará en el futuro.

- Un `fetch()` (petición asíncrona) a una **URL** para obtener un archivo `.json`.

- Un `new Audio()` de una **URL** con un `.mp3` al que se hace `.play()` para reproducirlo.

- Una comunicación desde Javascript a la API del sintetizador de voz del navegador.

- Una comunicación desde Javascript a la API de un sensor del smartphone.

Todos estos ejemplos se realizan mediante **tareas asíncronas**, ya que realizan un procedimiento que podría bloquear la ejecución del resto del programa al tardar mucho: la descarga de un fichero grande desde un servidor lento, una conexión a internet muy lenta, un dispositivo saturado a la hora de comunicarse con el sensor del celular, etc. 

Entonces... ¿Cómo gestionamos la asincronía? 

Teniendo en cuenta el punto anterior, debemos aprender a buscar mecanismos para dejar claro en nuestro código que ciertas tareas tienen que procesarse de forma asíncrona para poder quedarse esperando, pero otras deben hacerlo de forma síncrona. 

En JavaScript hay varias formas de gestionar la asincronía, donde quizás las más populares son las siguientes: 

| Herramienta   | Descripción                                                                                                                                                                             |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Callbacks     | Probablemente la forma más clásica (pero no sencilla) de gestionar la asincronía en JavaScript.                                                                                         |
| Promises      | Una forma más moderna y actual de gestionar la asincronía.                                                                                                                              |
| Async / Await | Seguimos con promesas, pero con async / await le agregamos más *syntactic sugar* (azúcar sintácica significa que en el fondo es lo mismo, pero buscando un código más corto y legible). |

¡Ahora sí! Veamos entonces cada uno de ellos.

# <span style="color: ivory">Callbacks</span>

### Utilizando asincronía con callbacks

Si recordamos lo visto recientemente, un callback es una función que podemos pasarle a otra función como parámetro para que ésta última pueda ejecutarla cuando lo necesite. Este mismo concepto se mantiene para los callbacks en asincronía.

Lo que buscamos aquí es tener una forma más legible de escribir funciones, más cómoda y flexible para reutilizarlas cuando lo necesitemos.

> Tengamos en cuenta que actualmente, controlar la asincronía sólo mediante callbacks puede ser una práctica un tanto obsoleta. Es preferible utilizar promesas o async/await, que generalmente es más adecuado y moderno.

Como dijimos recién, los **callbacks** no son más que un tipo de funciones que se pasan por parámetro a otras funciones. Además, los parámetros de dichas funciones toman un valor especial en el contexto del interior de la función.

Pero veamos un ejemplo. Imaginemos el siguiente bucle tradicional para recorrer un **array**:

```js
const list = ["A", "B", "C"];

for (let i = 0; i < list.length; i++) {
  console.log("i=", i, " list=", list[i]);
}</div>
```

En `i` tenemos la posición del **array** que estamos recorriendo (*va de 0 a 2*) y con `list[i]` accedemos a la posición del array para obtener el elemento, es decir, desde `A` hasta `C`. Ahora veamos, cómo podemos hacer este mismo bucle utilizando el método `forEach()` del **array** al cuál le pasamos una función callback:

```js
list.forEach(function(e,i) {
  console.log("i=", i, "list=", e);
});
```

Lo importante de este ejemplo es que se vea que la **función callback** que le hemos pasando a `forEach()` se va a ejecutar por cada uno de los elementos del array, y en cada iteración de dicha **función callback**, los parámetros `e`, `i` van a tener un valor especial:

- `e` es el elemento del array
- `i` es el índice (*posición*) del array

### Callbacks en JavaScript

Una vez entendido esto, vamos a profundizar un poco con las **funciones callbacks** utilizadas para realizar tareas asíncronas. Probablemente, el caso más fácil de entender es utilizar un temporizador mediante la función `setTimeout(``callback,` `time)`.

Dicha función nos exige **dos parámetros**:

- La función `callback` a ejecutar
- El tiempo `time` que esperará antes de ejecutarla

Así pues, el ejemplo sería el siguiente:

```js
setTimeout(function() {
  console.log("He ejecutado la función");
}, 2000);
```

Simplemente, le decimos a `setTimeout()` que ejecute la **función callback** que le pasamos por primer parámetro cuando transcurran `2000` milisegundos (*es decir, 2 segundos*). Utilizando arrow functions se puede simplificar el callback y hacer mucho más *fancy* y legible:

```js
setTimeout(() => console.log("He ejecutado la función"), 2000);
```

En cualquiera de los casos, lo importante es darse cuenta que estamos usando una **función callback** para pasársela a `setTimeout()`, que es otra función. En este caso, se trata de *programar* un suceso que ocurrirá en un momento conocido del futuro, pero muchas veces desconoceremos cuando se producirá (*o incluso si se llegará a producir*).

Si probamos el código a continuación, veremos que el segundo `console.log()` se ejecutará **antes** que el primero, dentro del `setTimeout()`, mostrando primero `Código sincrónico` y luego `Código asincrónico` en la consola del navegador:

```js
setTimeout(() => console.log("Código asíncrono."), 2000);
console.log("Código síncrono.");
```

El último `console.log` del código se ejecuta primero (*forma parte del flujo principal de ejecución del programa*). El `setTimeout()` que figura en una línea anterior, aunque se ejecuta antes, pone en espera a la función callback, que se ejecutará cuando se cumpla una cierta condición (*transcurran 2 segundos desde ese momento*).

### Asincronía con callbacks

Los **callbacks** pueden utilizarse como un primer intento de manejar la asincronía en un programa:

```js
function doTask(number, callback) {
  /* Código de la función */
}

doTask(42, function(err, result) {
  /* Trabajamos con err o result según nos interese */
});
```

Veamos que `doTask()` es la función que realiza la tarea en cuestión. Puede tener los parámetros que se consideren adecuados, como cualquier otra función, la diferencia es que establecemos un `callback` que usaremos para controlar lo que se debe hacer.

Más adelante, llamamos a la función `doTask()` y en su parámetro `callback` pasamos una función con dos parámetros; `err` y `result`. El primero de ellos, `err`, utilizado para controlar un error y el segundo de ellos, `result`, utilizado para manejar los valores devueltos.

---

# <span style="color: ivory">Promises</span>

Las **promesas** son un concepto para resolver el problema de asincronía de una forma mucho más elegante y práctica que, por ejemplo, utilizando *funciones callbacks* directamente.
Como su propio nombre indica, una **promesa** es algo que, en principio pensamos que se cumplirá, pero en el futuro pueden ocurrir varias cosas:
![Promises (Promesas Javascript)](https://lenguajejs.com/javascript/asincronia/promesas/promises.png)

- La promesa **se cumple** (*promesa resuelta*)
- La promesa **no se cumple** (*promesa se rechaza*)
- La promesa se queda en un **estado incierto** indefinidamente (*promesa pendiente*)

Con estas sencillas bases, podemos entender el funcionamiento de una **promesa** en Javascript. Antes de empezar, también debemos tener claro que existen dos partes importantes de las **promesas**: **cómo consumirlas** (*utilizar promesas*) y **cómo crearlas** (*preparar una función para que use promesas y se puedan consumir*).

### Promesas en Javascript

Las **promesas** en Javascript se representan a través de un **Objeto** , y cada **promesa** estará en un estado concreto: `pendiente`, `aceptada` o `rechazada`. Además, cada **promesa** tiene los siguientes métodos, que podremos llamar para utilizarla:

| Métodos                 | Descripción                                                              |
| ----------------------- | ------------------------------------------------------------------------ |
| `.then(resolve)`        | Ejecuta la función callback `resolve` cuando la **promesa** se cumple.   |
| `.catch(reject)`        | Ejecuta la función callback `reject` cuando la **promesa** se rechaza.   |
| `.then(resolve,reject)` | Método equivalente a las dos anteriores en el mismo `.then()`.           |
| `.finally(end)`         | Ejecuta la función callback `end` tanto si se cumple como si se rechaza. |

Más adelante veremos, que a diferencia del apartado anterior donde se utilizaban solamente funciones callback, en este enfoque se tiende a **no anidar promesas**, evitando así el famoso **Callback Hell**, y haciendo el código mucho más legible.

### Consumir una promesa

La forma general de consumir una **promesa** es utilizar el `.then()` con un sólo parámetro, puesto que muchas veces lo único que nos interesa es realizar una acción cuando la **promesa** se cumpla:

```js
fetch("/robots.txt").then(function(response) {
/* Código a ejecutarse cuando se cumpla la promesa */
});
```

Lo que vemos en el ejemplo anterior es el uso de la función `fetch()`, la cuál devuelve una **promesa** que se cumple cuando obtiene respuesta de la petición realizada. De esta forma, estaríamos preparando (*de una forma legible*) la forma de actuar de nuestro código a la respuesta de la petición realizada, todo ello de forma asíncrona.
Recordemos que podemos hacer uso del método `.catch()` para actuar cuando se rechaza una **promesa**:

```js
fetch("/robots.txt")
.then(function(response) {
  /* Código a ejecutarse cuando se cumpla la promesa */
})
.catch(function(error) {
  /* Código a ejecutarse cuando se rechaza la promesa */
});
```

Miremos cómo hemos indentado los métodos `.then()` y `.catch()`, ya que se suele hacer así para que sea mucho más legible para quien los lee. Además, se pueden encadenar varios `.then()` si se siguen generando **promesas** y se devuelven con un `return`:

```js
fetch("/robots.txt")
.then(response => {
  return response.text(); // Devuelve una promesa
})
.then(data => {
  console.log(data);
})
.catch(error => { /* Código a realizar cuando se rechaza la promesa */ });
```

De hecho, usando **arrow functions** se puede mejorar aún más la legibilidad de este código, recordando que cuando sólo tenemos una sentencia en el cuerpo de la **arrow function** hay un `return` implícito:

```js
fetch("/robots.txt")
.then(response => response.text())
.then(data => console.log(data))
.finally(() => console.log("Terminado."))
.catch(error => console.error(data));
```

Veamos también que además hemos añadido el método `.finally()` para agregar una función callback que se ejecutará **tanto si la promesa se cumple como si se rechaza**, lo que nos ahorrará tener que repetir la función en el `.then()` y en el `.catch()`.

> En toda esta sección hemos visto cómo utilizar o consumir una **promesa** haciendo uso de `.then()`, que es lo que en la mayoría de los casos necesitaremos. Sin embargo, vamos a ver en el siguiente apartado como crear o implementar nuestras propias **promesas** para su posterior consumo.

### Asincronía con promesas

Vamos ahora a implementar el ejercicio base que hemos comentado en el primer capítulo de este tema utilizando **promesas**. Observa que lo primero que haremos es crear un nuevo objeto **`Promise`** que *envuelve* toda la función de la tarea `doTask()`.
Al `new Promise()` se le pasa por parámetro una función con dos callbacks, el primero es `resolve`, que utilizaremos cuando se cumpla la promesa; y el segundo es `reject`, para cuando se rechace:

```js
/* Implementación con promesas */
const doTask = (iterations) => new Promise((resolve, reject) => {
const numbers = [];
for (let i = 0; i < iterations; i++) {
  const number = 1 + Math.floor(Math.random() * 6);
  numbers.push(number);
  if (number === 6) {
    reject({
      error: true,
      message: "Se ha sacado un 6"
    });
  }
}
resolve({
  error: false,
  value: numbers
});
});
```

Como vemos, se trata de una implementación muy similar a los callbacks que vimos en el apartado anterior, pero utilizan una **promesa** nativa para poder luego consumirla cómodamente:

```js
doTask(10)
.then(result => console.log("Tiradas correctas: ", result.value))
.catch(err => console.error("Ha ocurrido algo: ", err.message));
```

Imaginemos el caso de que **cada lanzamiento del dado** (*la parte donde genera el número aleatorio*) fuera un proceso más costoso que **tardara un tiempo considerable**, quizás de esa forma se vea más clara la necesidad de una tarea asíncrona, controlada con **promesas**.

---

# <span style="color: ivory">Async / Await</span>

En el 2017 se introdujeron las palabras clave `async` y `await`, que no son más que una forma de **syntactic sugar** para gestionar las promesas de una forma más sencilla. Con `async` y `await` seguimos utilizando promesas, pero abandonamos el modelo de encadenamiento de `.then()` para utilizar uno en el que trabajamos de forma más tradicional. 

### La palabra clave async

En primer lugar, tenemos la palabra clave `async`. Esta palabra clave se colocará previamente a `function` en caso de funciones tradicionales, y previo a los paréntesis para funciones flecha; para definirla así como una **función asíncrona**, el resto de la función no cambia:

```js
async function funcion_asincrona() {
  return 42;
}
```

```js
const funcion_asincrona = async () => 42;
```

Al ejecutar la función veremos que ya nos devuelve una **promesa** que ha sido cumplida, con el valor devuelto en la función (*en este caso, 42*). De hecho, podríamos utilizar un `.then()` para manejar la **promesa**:

```js
funcion_asincrona().then(value => {
  console.log("El resultado devuelto es: ", value);
});
```

Sin embargo, veremos que lo que se suele hacer junto a `async` es utilizar la palabra clave `await`, que es donde reside lo interesante de utilizar este enfoque.

### La palabra clave await

Cualquier función definida con `async`, o lo que es lo mismo, cualquier **promesa** puede utilizarse junto a la palabra clave `await` para manejarla. Lo que hace `await` es esperar a que se resuelva la **promesa**, mientras permite continuar ejecutando otras tareas que puedan realizarse:

```js
const funcion_asincrona = async () => 42;
const value = funcion_asincrona();             // Promise { <fulfilled>: 42 }
const asyncValue = await funcion_asincrona();  // 42
```

Observa que en el caso de `value`, que se ejecuta sin `await`, lo que obtenemos es el valor devuelto por la función, pero *envuelto* en una **promesa** que deberá utilizarse con `.then()` para manejarse. Sin embargo, en `asyncValue` estamos obteniendo un tipo de dato , guardando el valor directamente ya procesado, ya que `await` espera a que se resuelva la **promesa** de forma asíncrona y guarda el valor.

### Asincronía con async/await

Volvamos al ejemplo quevimos en los anteriores capítulos. Recordemos que la función `doTask()` realiza 10 lanzamientos de un dado y nos devuelve los resultados obtenidos o detiene la tarea si se obtiene un 6.
La implementación de la función sufrirá algunos cambios, simplificándose considerablemente. En primer lugar, añadimos la palabra clave `async` antes de los parámetros de la **arrow function**. En segundo lugar, desaparece cualquier mención a **promesas**, se devuelven directamente los objetos, ya que al ser una función `async` se devolverá todo envuelto en una **promesa**:

```js
const doTask = async (iterations) => {
  const numbers = [];
  for (let i = 0; i < iterations; i++) {
    const number = 1 + Math.floor(Math.random() * 6);
    numbers.push(number);
    if (number === 6) {
      return {
        error: true,
        message: "Se ha sacado un 6"
      };
    }
  }
  return {
    error: false,
    value: numbers
  };
}
```

Pero donde se introducen cambios considerables es a la hora de consumir las **promesas** con `async`/`await`. No tendríamos que utilizar `.then()`, sino que podemos simplemente utilizar `await` para esperar la resolución de la **promesa**, obteniendo el valor directamente:

```js
const resultado = await doTask(10);   // Devuelve un objeto, no una promesa
```

> Recuerda que en el caso de querer controlar errores o promesas rechazadas, siempre podrás utilizar bloques `try`/`catch`.

### Para cerrar este módulo

Excelente! Hemos visto cómo trabajar con asincronía de varias maneras, de aquí en adelante siempre priorizaremos utilizar la metodología más moderna y legible, es decir, **async/await**.
