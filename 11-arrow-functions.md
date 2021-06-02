# ¿Qué son Arrow functions?

Entrega número 12 del primer curso de MicroBytes, ya nos estamos acercando al final y la oportunidad de iniciar un nuevo proceso!.

Hoy te invito a revisar: **Arrow functions** (funciones flechas), ¿que son?, ¿por que y cómo?.

**this**, ¿lo conoces?. Un concepto o dudosa característica del lenguaje que puede ser lo suficientemente complejo como para ser pregunta de entrevista, y tema de cientos de videos en youtube. Tal es el desagrado de utilizar este concepto que la comunidad buscó nuevas formas de trabajar agregando nuevas opciones: **Arrow functions**.

```javascript
const hola = () => 'Hola MicroBytes';
```

En Javascript, las funciones son “objetos de primera clase” o dicho de otra forma puedes manipularlas y “transmitirlas” como cualquier otro objeto o variable.

También cabe destacar que una función es la mejor forma de encapsular lógica, y por ende, compartirla, una función siempre retorna un valor, si no se define entonces será `undefined`.

```javascript
function hola() {
	return 'Hola MicroBytes'
}

console.log(hola());
```

Esa es una función “clásica”, que también puede ser escrita en forma de expresión al asignar la función a una variable

```javascript
const hola = function() {
	return 'Hola MicroBytes';
}
console.log(hola())
```

Este ejemplo tiene cierto parecido a ese snippet de código inicial. Una arrow function es una expresión que:

- Puede ser escrita en una línea de código (si es lo suficientemente simple).
- no necesitas escribir la palabra clave **function:** De cierta forma la flecha `=>` reemplaza la palabra clave.
- no necesitas escribir **return:** tiene retorno implícito.
- no necesitas escribir las llaves **{}**

## ¿Cómo usarlas?

Las arrow functions tienen varias “alternativas” a la hora de escribirlas, opciones que buscan simplificar su uso y aumentar su capacidad de expresión.

### No más return, o casi.

Un arrow function no requiere del uso de la palabra clave `return` pero sólo en algunos casos: Cuando el cuerpo de la misma es sólo una expresión.

```javascript
const adultUsers = users.filter(user => user.age > 18).map(user => user.name)
```

Sin retorno implícito el ejemplo se vería:

```javascript
const adultUsers = users.filter(user => {
	return user.age > 18
}).map(user => {
	return user.name
})
```

Siempre es bueno tener en cuenta que:

- **Solo funciona para expresiones sencillas**, si hay más de una declaración entonces debes usar las llaves **{}** y la palabra **return.**
- **Si retornas un objeto** debes tener cuidado y usar paréntesis extras.

```javascript
// Esto fallará: Uncaught SyntaxError: unexpected token: ':'
const adultusers = users.map(user => { ...user, date: Date.now() })

// Esto si funciona
const adultusers = users.map(user => ({ ...user, date: Date.now() }))
```

### Olvida los paréntesis de los argumentos… pero no siempre

**Sí tu función tiene sólo un parámetro** puedes no escribir los paréntesis al rededor de él.

```javascript
const holaMundo = name => `Hola ${name}`
```

Pero si necesitas que este único parámetro tengo un valor por defecto, entonces los paréntesis deben volver.

```javascript
const holaMundo = (name = "Matias") => `Hola ${name}`
```

### No tiene nombre!

En los ejemplos anterior pudimos ver el uso de estas funciones anónimas, sobre todo en las funciones callback de los métodos de arreglo.

> Una función anónima, también conocida como "lambda" en otros lenguajes es una función que no está atada a un identificador

Todas las arrow functions son anónimas. Es posible asociar una función anónima a una variable - que no es lo mismo que nombrar la función -

### Adios `arguments` .

Recuerdas en una entrega anterior hablamos de este truco para obtener todos los argumentos de una función?. Bueno, no funciona con arrow functions.

```javascript
function withArguments() {
	console.log(arguments)
}

withArguments(1,2,3); // [1,2,3]

// usando arrow functions

const withArguments = () => {
	console.log(arguments); //Uncaught ReferenceError: arguments is not defined
}
```

Así que si de verdad necesitas el argumento `arguments`, entonces necesitas usar funciones clásicas.

> Pero, ¿De que otra forma podemos conseguir un comportamiento similar al uso de `arguments`?

### Al fin podemos olvidarnos de this

En estas funciones el valor de `this` se mantiene sin cambios, es decir, siempre hace referencia al ámbito padre, esto es conocido como **lexical scoping,**

> **lexical scoping:** Una forma de definir el *ámbito* de una variable para permitir que esta sea sólo accesible o llamada dentro del mismo bloque donde fue definida.

En un ejemplo

```javascript
document.querySelector('#submit').addEventListener('click', function() {
	console.log(this); //	<button id="submit">
})

document.querySelector('#submit').addEventListener('click', () => {
	console.log(this); // 	Window
})
```

¿Por qué en el  segundo ejemplo, el `console.log(this)` nos muestra el objeto `Window`? Por que estamos usando una arrow function en donde `this` no se vuelve a "enlazar”, si no que hacer referencia al ámbito padre, en este caso, `document.window`.

Las arrow function heredan el valor de `this.`

## ¿Cómo se relaciona con React?

Es un patrón muy común utilizar arrow functions dentro de JSX para renderizar un `map`, `filter` u otro método de arreglo.

```javascript
function AwesomeList({data}) {
  return (
	<ul>
	  {data.map(item => (
		<li key={item.id}>
		  <span>{item.name}</span>
		</li>
	  ))}
	</ul>
  )
}
```

Notar que en este ejemplo usamos el “retorno implícito” parar retornar el componente JSX `li`. No esta escrito en una sola línea pero cumple con ser una sola declaración.

## Conclusión

Usar arrow functions es la preferencia actual al escribir Javascript moderno, estas te permiten escribir métodos más concisos o simplificaciones de una linea mas legibles aprovechando las características de retorno implícito y el no uso de paréntesis.

Además te permite olvidarte de manejar el contexto `this` (algo que por cierto no vemos mucho en React).
Este tipo de funciones es muy usada como argumento para los métodos de arreglos como `.map()`, `.filter()` , `reduce()` y otros.

