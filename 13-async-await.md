# Async/Await

Penúltima entrega de Microbytes. Esta semana revisaremos un poco de “azúcar sintáctica”. Async/Await .

La semana pasada revisamos que son las promesas y como trabajar con ellas, a modo de recordatorio

```javascript
const sleep = time => new Promise((resolve, reject) => setTimeout(resolve, time));

sleep(3000).then(() => {
	console.log('¿Funcionó la promesa?')
})
```

Esta función `sleep` retorna una promesa, para ejecutar "algo" cuando la promesa es resuelta utilizamos `then` y pasamos una función.

En modo general puedes considerar una función que retorna una promesa como una función asíncrona. Pero, esto no es efectivamente declarado en el código, si no más bien inferido dado que la función retorna una promesa. ¿Qué tal si tuvieras una manera de declarar que una promesa es asíncrona?.

## Funciones asíncronas.

> Como siempre, encuentra el [demo de este artículo aquí](https://jsitor.com/29TZ5Wnls4)

Una función asíncrona es aquella declarada utilizando la palabra clave `async`. Esta puede ser usada en funciones normales o en arrow functions.

```javascript
async function doSomething() {
}

const doSomething = async () => {
}
```

¿Qué significa esta declaración `async`?

Básicamente lo que estás haciendo es indicándole al intérprete que la función `doSomething` retornará **siempre** una promesa.

```javascript
const doSomething = async () => {
	return 1
}

// es lo mismo

const doSomething = () => new Promise(resolve => resolve(1))

// ambas se usan igual

const promise = doSomething()
promise.then(result => {
	console.log(reuslt)
})
```

Pero, a pesar de ser semánticamente lo mismo para el interprete el uso de `async` nos ofrece una nueva característica

## `await`

El uso de `async` para declarar tu función te permite usar una nueva palabra clave: `await` que de forma muy resumida, te permite literalmente esperar a que la promesa esté resuelta.

```javascript
// sin async/await
const getData = () => {
  console.log('Fetching...')
  fetchData().then(result => {
	// hacer algo con el resultado
	console.log(result)
	return result
  })
}

// usando async/await

const asyncGetData = async () => {
  console.log('Fetching async....')
  const result = await fetchData()
  console.log(result)
}
```

En el ejemplo tenemos la misma implementación, primer con promesa y luego con `async/await` la diferencia es clara, el uso de `await` permite una mejor legibilidad. Básicamente, `await` es un reemplazo del código que escribes dentro de la llamada a `then` en una promesa y te permite escribir código de forma síncrona.

Pero, tu bien sabes que una promesa puede ser resuelta o rechazada y notaste que `await` solo determina el camino feliz de una promesa resuelta con éxito. ¿Que ocurre con los errores?.

## Manejo de errores

El objeto Promise nos ofrece 3 métodos que te permiten controlar el flujo de la promesa o “suscribirte a los cambios de estado”, estos son `then`, `catch` y `finally`.

`then` se ejecuta en caso de una promesa exitosa y es lo que conseguimos al utilizar `await`.

`catch` define el trozo de código que se ejecutará si la promesa fue rechazada, es decir, tu manejo de errores. En el caso de `async/await` no tenemos la misma definición pero, podemos decantar al nunca bien ponderado bloque `try

`catch` define el trozo de código que se ejecutará si la promesa fue rechazada, es decir, tu manejo de errores. En el caso de `async/await` no tenemos la misma definición pero, podemos decantar al nunca bien ponderado bloque `try/catch`

```javascript
const asyncGetDataWithTry = async () => {
  console.log('Fetching async....')
  try {
	const result = await fetchData()
	console.log('Async Data: ', result)
  }catch(e) {
	console.log(e)
  }  
}
```

El proceso completo de la promesa se ejecuta dentro del bloque `try` y si esta promesa llegase a falla, el bloque `catch` se encarga de ella.

El bloque `finally` es lo que puedes escribir directamente fuera del bloque `try/catch.`

Ahora, ¿qué ocurre si tienes varias promesas y varios bloque `try/catch` ?

```javascript
const test = async () => {
  try {
	const one = await getOne(false)
  } catch (error) {
	console.log(error)
  }

  try {
	const two = await getTwo(true)
  } catch (error) {
	console.log(error)
  }

  try {
	const three = await getThree(false)
  } catch (error) {
	console.log(error)
  }
}
```

Eso no se ve nada bien.

¿Qué puedes hacer?

Una forma de mejorar la legibilidad de esto es simplemente poner todo dentro de un sólo bloque `try/catch`

```javascript
const test = async () => {
  try {
	const one = await getOne(false)
	const two = await getTwo(false)
	const three = await getThree(false)
  }catch (error) {
	console.log(error) // Failure!
  }
}
```

idealmente el objeto error retornado por las promesas tiene algún código identificador lo que te permite identificar cual fue el error.

> ¿De que otra forma puedes escribir el manejo de errores de múltiples funciones asíncronas?
Cuéntame tu solución respondiendo a este email o en un tweet 😎

# Conclusión

En resumen `async/await` es una notación o sintaxis que busca simplificar el proceso de escribir (y sobre todo leer y mantener) código asíncrono transformado el uso de promesas y callbacks en código que se puede seguir de forma síncrona, para esto `await` espera por el resultado de la promesa deteniendo la ejecución hasta que la promesa se resuelva - el código justo después de `await` no se ejecuta hasta que la promesa esté lista - .

> No olvides que no puedes usar `await` si no has declarado la función como asíncrona utilizando `async`.

# Desafío

Imagina que tienes que ejecutar múltiples llamadas asíncronas a una API externa, cada llamada a la API demora casi 1 segundo.

```javascript
const test = async _ => {
  const one = await fetchOne()
  console.log(one)

  const two = await fetchTwo()
  console.log(two)

  const three = await fetchThree()
  console.log(three)

  console.log('Done')
}
```

Este código resuelve el problema pero crea otro: Cada llamada espera que la anterior termine, y si cada llamada demora 1 segundo, entonces el proceso estará listo solo después de 3 segundos!!.

Y por lo que este código muestra, las llamadas no son dependientes entre si. 
¿Cómo puedes escribir este código pero ejecutando las 3 llamadas en paralelo?

Pista: El objeto `Promise` tiene un método que te permitirá resolver este problema.

