# Promesas, sólo promesas

Ya estamos llegando casi al final de este primer micro-curso gratuito de MicroBytes.
Gracias por ser parte de este creciente newsletter.

En esta entrega revisaremos como trabajar con promesas.

# ¿WTF son las promesas?

**Promises es es un objeto para manipular acciones asíncronas dentro de Javascript.**

Javascript es un lenguaje que se ejecuta en el browser con la idea de dar dinamismo al contenido ahí desplegado, esto implica que muchas operaciones se deben dar de forma asíncrona. Dado que los motores de javascript son "Single Threaded", es decir, Javascript no maneja múltiples hilos es necesario implementar otra estrategia para la asincronía.

una Promise es un objeto que produce un valor que estará disponible en algún momento futuro.

El concepto de promesa, **future** o **deferred** es una construcción utilizada para “sincronizar” la ejecución de instrucciones en lenguajes concurrentes. Este concepto describe un objeto cuyo valor inicial es desconocido.

Este concepto fue acuñado hace ya varios años, nacidas (como muchos de los conceptos “cool” que usamos hoy) de la programación funcional, siendo quizá el primer lenguaje en implementar estas ideas **MultiLisp.**

El desarrollo web trajo nueva vida al concepto dada su naturaleza asíncrona y el modelo de `request-response` utilizado. (solicitar y obtener datos mediante una red donde el solicitante queda a la "espera" de una respuesta).

> Si te interesa saber más del concepto de **promises y futures** puedes encontrar el [paper original aquí](https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.295.9692&rep=rep1&type=pdf)

> O [esta sección sobre concurrencia](https://www.braveclojure.com/concurrency/) del libro “Clojure for the Brave and True”

Las **Promises** como hoy las conocemos no llegaron a Javascript hasta la versión ES6, antes de eso todo era un caos y cada quien implementaba comportamientos asíncronos como mejor le pareciera 🤯

## ¿Como funcionan las promesas?

Un promesa es un objeto, y como tal tiene varios atributos y métodos que nos permiten trabajar con el. La idea de este objeto es poder acceder y trabajar con el de forma `sincrona` pero cuyo valor es  `asincrono` . Un gato de Schrödinger.

Una promesa tiene 3 estados.

- **Pending:** Es el valor inicial, indicando que está a la espera de que “el trabajo” sea realizado.
- **Fullfilled**: Cuando la operación se ejecuta de forma existosa.
- **Rejected:** Cuando la operación falla de alguna manera.

Ahora bien, estos estados no son expuestos como tal, una promesa es más bien “una caja negra” en donde sólo la función que creo la promesa tiene conocimiento de su propio estado.

### ¿Cómo se crea una promesa?

Suficiente texto, algo de código.

```javascript
const sleep = time => new Promise((resolve, reject) => setTimeout(resolve, time));

sleep(3000)

console.log('¿Funcionó la promesa?')
```

**¿Qué ocurre al ejecutar este código?**

Nada fuera de lo esperado 😅.

En este código creamos una función `sleep` que recibe el parámetro `time`, un número y retorna una promesa.

Para crear una promesa usamos su constructor y la palabra reserva `new`. La promesa recibe una función tipo `callback` como único argumento. Esta función recibe dos parámetros. `resolve` y `reject`.

Estas funciones `resolve` y `reject` son funciones pre-definidas por el motor de Javascript para manejar los estados de la promesa, nosotros sólo debemos utilizarlas cuando corresponde.

- `resolve` Es utilizado para marcar que el proceso de la promesa terminó de forma exitosa. `resolve` acepta un parámetro que es retornado como resultado de la ejecución.
- `reject` Se usa para determinar cuando el proceso falló, recibe un argumento que indica la razón del **rechazo** de la promesa.

Entonces, en el código, nuestra función callback es

```javascript
(resolve, reject) => setTimeout(resolve, time)
```

Donde, `time` es el argumento de la función externa (👋 closures otra vez). `setTimeout` ejecutará la función que se usa como argumento cuando el tiempo definido pase, es decir cuando `time` se cumpla. La función que pasamos como argumento es `resolve`, entonces `setTimeout` ejecutará `resolve` cuando el tiempo indicado termine.

Esta promesa en particular no falla jamas, por lo que podemos reescribirla así

```javascript
const sleep = time => new Promise((resolve) => setTimeout(resolve, time));
```

Simplemente, eliminando el argumento `reject`.

> **Solo puedes utilizar un sólo resultado o error dentro de una promesa.**

> La función "ejecutora" sólo puede tener una llamada **y solo una** a `resolve` o una a  `reject`. Estas funciones cambian el estado de la promesa y este cambio de estado es único e irreversible.

> **Se recomienda rechazar con errores**

> El método  `reject` acepta cualquier valor (al igual que `resolve`) pero dado el contexto de  "rechazo”, se recomienda usar objetos `Error` para indicar la razón del rechazo.

**Ya, pero ¿Por qué el `console.log` no se ejecuta después de 3 segundos?**

Esto es por que nuestro código no indica que se espere el resultado de la promesa.

Podemos pensar en la promesa como un enlace entre a función asíncrona encerrada en la caja negra y el código que necesita consumir su resultado. Para esto, `Promise` permite registrar o "suscribirse" a la espera del resultado usando los métodos `.then`, `.catch`,  `.finally`.  Una `Promise` es un objeto  `theneable`.

### .then

El método `.then`  es el más importante y fundamental, es el que nos permite "leer” el contenido de la caja negra, su sintaxis es

```javascript
promise.then(
	function(result) {},
	function(error) {}
);
```

Es decir, recibe dos argumentos tipo función para manejar cada uno de los posibles estados de la promesa: `Fullfilled` y `Rejected` .

El primer argumento es una función que se ejecutará cuando la promesa es resuelta existosamente, es decir, cuando se usa `resolve`. Esta función recibe como argumento el valor pasado al método `resolve`.

El segundo argumento, es otra función pero que esta vez se ejecuta cuando la promesa es rechazada, y recibe como parámetro la razón de ese rechazo: el valor pasado a  `reject`.

Entonces, ahora podemos modificar nuestro ejemplo utilizando `.then`

```javascript
const sleep = time => new Promise((resolve, reject) => setTimeout(resolve, time));

sleep(3000).then(() => {
	console.log('¿Funcionó la promesa?')
})
```

Ahora, concatenamos la ejecución de `console.log` a la espera de que la promesa `sleep` se cumpla y en ese momento se muestra el mensaje. Y como en nuestro caso `resolve` no recibe ningún valor, nuestro función de "èxito" tampoco recibe parámetros y solo se ejecuta.

Es posible separar el manejo de errores y de ejecuciones exitosas para dar un poco de legibilidad al código, para eso podemos sólo usar el primer argumento de `.then` y luego usar el siguiente método: `.catch`

### .catch

Este método sólo se ejecuta cuando la promesa fue rechazada usando  `reject`. Este método es un análogo de usar `.then(null, f)` (siendo `f` una función), existe solo por claridad de su ejecución.

¿Pero que ocurre si quieres ejecutar algún proceso sin importar si la promesa se cumplió o no?. Entra `.finally`

### .finally

Muy similar a como funciona `.finally` en un bloque `try/catch` este ejecutará siempre la función que se le pase como argumento, independiente del resultado de la promesa.

- La función utilizada dentro de `finally` no recibe argumentos. No sabemos si la promesa fue exitosa o no.
- Puedes concatenar otros “manejadores” después de `finally`.

```javascript
const sleep = time => new Promise((resolve, reject) => setTimeout(resolve, time));

sleep(3000)
.finally(() => console.log('Promesa está lista'))
.then(() => {
	console.log('¿Funcionó la promesa?')
})
.catch(err => {
	console.log(erro)
})
```

### Algunas “reglas”

- Una promesa es `thenable`, el objeto siempre otorga un método `then`
- Una promesa pendiente puede transicionar al estado `fullfilled` or `rejected`
- Una promesa en estado `fullfilled` o  `rejected` se conoce como `settled` y no puede cambiar de estado nuevamente.

### Encadenamiento

Dado que `.then` siempre retorna una nueva promesa, es posible encadenar promesas otorgando mas control sobre como se manejan los resultados y errores, un ejemplo muy utilizado en el mundo real es trabajando con la api `fetch`.

La [api `fetch`](https://developer.mozilla.org/es/docs/Web/API/Fetch_API) es la interfaz nativa de javascript para obtener recursos externos y funciona como una Promesa.

```javascript
const obtenerDatos = () => {
	return fetch('/algunaUrl/)
}

obtenerDatos()
		.then(response => response.json())
		.then(json => json)
		.then(print => {
			console.log(print)
		})
		.catch(error => console.error(error)
```

En este ejemplo creamos una función que utiliza la api `fetch` para obtener datos de algún endpoint y retorna una promesa. Podemos entonces usar `.then` para leer la respuesta obtenida.

La respuesta de una llamada a `fetch` es un objeto `Response` que tiene un método que permite obtener el cuerpo de la respuesta en forma json y que es en si mismo una promesa.

`.then(response => response.json())`

Usamos el retorno implícito de la arrow function para retornar inmediatamente la promesa. Luego volvemos a encadenar usando `then` y obtenemos el valor retornado previamente

`. then(json => json)`

En este punto, podemos ya utilizar el valor, pero solo por propositos de demostración, lo retornamos `json => json` y volvemos a encadenar `.then(print => {})` creando una nueva función que se encarga de imprimir el resultado.

Finalmente encadeamos un `.catch` para capturar cualquier error.

## Algunos otros métodos.

El objeto `Promise` nos ofrece algunos otros métodos "utilitarios”:

- `Promise.all` permite obtener el resultado de un grupo de promesas. Recibe como argumento un `iterable` como un arreglo de promesas y espera a que todas las promesas se cumplan o a que una sea rechazada para retornar. Si todas las promesas se cumplen, retornará al siguiente`.then` un arreglo con los resultados en el mismo orden que las promesas pasadas como argumento.
- `Promise.race` También recibe un arreglo de promesas pero retorna tan pronto **una** promesa se cumple o rechaza.
- `Promise.allSettled` También recibe un `iterable` de promesas y espera a que todas las promesas hayan terminado sin importar si fueron exitosas o rechazadas. Retorna una promesa con un arreglo de objetos con los resultados de cada promesa pasada como argumento.
- `Promise.any`. Recibe un `iterable` y retorna tan pronto una de las promesas termina exitosamente.

> **Desafío**

> Escribe una promesa que te permita cargar un script de forma asíncrona y te comunique cuando se ejecuto exitosamente o falló. Puedes usar el [código base disponible aquí.](https://jsitor.com/60HE9qayf)

## ¿Cómo se relaciona con React?.

Usarás promesas en cada momento con o sin React. Finalmente nuestro trabajo es crear soluciones que implican al menos el 99% de las veces desplegar información que proviene de algún servicio externo lo que implica asincronicidad y múltiples estados, siendo las promesas la forma de manejar esa situación.

En React es posible te encuentres con algo así para obtener datos que mostrar dentro de un componente.

```javascript
const Componente = () => {
	const [isLoading, setIsLoading] = React.useState(false)
	const [error, setError ] = React.useState(null)
	const [data, setData] = React.useState(null)
	
	React.useEffect(() => {
		setIsLoading(true)

		// Promesa
		fetch(ALGUNA_URL)
		.then(response => response.json())
		.then(result => setData(result)
		.catch(error => {
			setError(error)
		})
		.finally(() => {
			setIsLoading(false)
		})
	}, [setIsLoading, setData]

	return ........
}
```

En este ejemplo se busca obtener datos desde `ALGUNA_URL` y se encadenan varios `.then`,`.catch` y `.finally` para que el componente responda  a los diferentes estados de la promesa.

