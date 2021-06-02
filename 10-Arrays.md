# Arreglos, Arreglos, Arreglos

Ahora que ya revisamos inmutabilidad y nos sacamos esa definición y los métodos relacionados de encima podemos volver al tema de revisar características de Javascript que nos serán muy útiles en nuestra caja de herramientas a la hora de trabajar con React, o con cualquier framework moderno.

Hoy revisaremos distintos **métodos de arreglos**.

## ¿Qué es un arreglo?

Los arreglos, son una sencilla estructura de datos basada en indices, que podemos crear al escribir `[]` , son parte esencial en cualquier aplicación ya que casi todo tipo de datos que queramos mostrar en la interfaz será representado por una colección.

En general un arreglo `Array` es representado por una sola variable en donde puedes almacenar múltiples y diferentes elementos.

```javascript
const array = [1,2,3,'cuatro', { number: 5} ]

console.log(array[3]); // "cuatro"
```

En este pequeño ejemplo se muestra que un arreglo puede almacenar cualquier tipo de elementos “al mismo tiempo”. Y en la siguiente linea se muestra como acceder a un elemento del arreglo utilizando un indice numérico.

## Los métodos

El objeto Arreglo tiene varios métodos que permiten manipular su contenido. Estos métodos son parte del prototipo por lo que puedes acceder a ellos desde cualquier instancia.

Nos enfocaremos en los métodos inmutables que en mi experiencia son los que más utilizarás en tu día a día desarrollando aplicaciones.

Todos estos métodos reciben como argumento una función tipo “callback” que el método ejecutará cada vez que itera sobre los elementos del arreglo. En esta función callback definimos lo que queremos lograr pero no indicamos el cómo lograrlo, cada método simplemente aplica nuestra lógica, es decir **son métodos declarativos**.

Revisaremos: `Array.find`, `Array.some`, `Array.every`, `Array.includes`, `Array.filter`, `Array.map` y `Array.reduce

En todos los ejemplos trabajaremos con esta estructura inicial.

```javascript
const users = [{
	id: 'efe5f844-788f-42e3-8706-ae7312958576',
	username: 'Justin Elliott',
	twitter: '@justinElliot',
	email: 'justin.elliott@example.com'
  },{
	id: '309b8b06-b5f5-42a2-9808-8bda5da90fb9',
	username: 'Paul C Wiggins',
	twitter: '@RealPaul',
	email: 'paul.wiggins@example.com'
  },{
	id: '47b793e4-d1cd-4ff7-8f85-37c5c3268fc0',
	username: 'Margaret J Pitre',
	twitter: '@mpitre',
	email: 'margaret.pitre@example.com'
  }, {
	id: 'a34b752c-2ac7-420e-ab0b-8ccf8d18deb6',
	username: 'Sharon J Jenkins',
	twitter: null,
	email: 'sharon.jenkins@example.com'
  }
]
```

## Array.some y Array.every

Estos métodos son parecidos pero opuestos, ambos retornan un valor booleano `true` o `false` si se cumple o no la condición definida.

`some` retornará `true`si al menos uno de los elementos del arreglo **"cumple**” la prueba.

`every` retorna `true` sólo cuando todos los elementos del arreglo "**cumplen**" con la función de prueba.

```javascript
const isThereAnyJustin = users.some(item => item.username.includes('Justin'))
console.log({ isThereAnyJustin }) // true
```

```javascript
const doAllHaveTwitter = users.every(item => item.twitter)
console.log({ doAllHaveTwitter })

const doAllHaveUsername = users.every(item => item.username)
console.log({ doAllHaveUsername })
```

En el primer ejemplo, la función de prueba revisa si la propiedad `username` del ítem actual incluye la palabra `Justin`.

En el segundo ejemplo podemos ver dos usos de `Array.every` a modo de verificación.

## Array.find

¿Qué haces si quieres encontrar un elemento dentro de un arreglo?. Como es un caso de uso tan común, tenemos un método para lograrlo.

Este método busca y retorna el primer elemento dentro del arreglo que cumple con una determinada condición, si es `true`, se detiene la búsqueda y se retorna el elemento, en caso contrario se continua buscando y si no se encuentra se retorna `undefined`

```javascript
onst id = '309b8b06-b5f5-42a2-9808-8bda5da90fb9';
const id2 = 'b5f5-42a2-9808-8bda5da90fb9-309b8b06'

const searchItem = (id) => item => item.id === id

const myUser = users.find(searchItem(id))
console.log('Found: ', myUser)

const myUser2 = users.find(searchItem(id2))
console.log('Not found', myUser2)
```

> ¿**De que otra forma  puedes escribir la función `searchItem`?**

> Envíame tu respuesta, o publícala en un gists!

## Array.includes

Es casi idéntico al método `String.prototype.includes` y en efecto hacen lo mismo determinar si un elemento, en este caso un carácter o sub-string está dentro del String, en particular este método retorna `true` o `false` si el arreglo contiene o no un elemento.

> `includes` hace una comparación por **referencia** por lo que no es útil para revisar si un arreglo tiene o no un objeto.

```javascript
const letters = ['a','B','c','D','E']
const haveI = letters.includes('i')
const haveE = letters.includes('E')
console.log({ haveI, haveE })
```

En este ejemplo, tenemos un arreglo que contiene letras y dos constantes, `haveI` y `haveE` que revisan si el arreglo contiene o no una letra en particular.

## Array.map

`Array.map` y los siguientes métodos son las construcciones mas poderosas para manipular arreglos.

`Array.map` es usado para modificar la forma (pero no el tamaño) de un arreglo.

Con la función callback que pasas como parámetro tienes acceso a cada item del arreglo para así manipularlo, la función puede recibir hasta 3 argumentos:

- `item`: El item actual de la iteración.
- `index`: El indice del elemento actual.
- `array`: el arreglo sobre el que fue llamado.

```javascript
const newUsers = users.map(item => {
  return {
	twitter: item.twitter
  }
})
console.log(newUsers)
```

En este ejemplo “filtramos” las propiedades que no nos interesan y dejamos sólo la propiedad `twitter`.

También podemos agregar propiedades

```javascript
const newUsers2 = users.map(item => {
  return {
	...item,
	date: (new Date()).toLocaleDateString()
  }
})
console.log(newUsers2)
```

```javascript
const newUsers3 = users.map(item => {
  if(item.twitter) {
	return {
	  ...item,
	  date: (new Date()).toLocaleDateString()
	}
  } 
  
})
console.log(newUsers3)
```

En este ejemplo, se muestra un bloque condicional que retorna el nuevo objeto cuando es `true` pero no retorna en caso contrario, creando así un elemento `undefined` en el resultado.

## Array.filter

Dado el caso anterior, en que tenemos un elemento `undefined` en el nuevo arreglo: ¿Cómo podemos eliminarlo?

```javascript
const filtrado = newUsers3.filter(item => item)
console.log({ filtrado })
```

La función callback de `Array.filter` debe retornar `true` para considerar que el elemento pasó la prueba y sea "agregado" al arreglo final.

¿Recuerdas los conceptos `truthy` y `falsy`?. Aquí hacemos uso de ese concepto al retornar directamente el ítem, si este es `undefined` será considerado `falsy`.

## Array.reduce

La definición oficial de este método en el sitio de mozilla MDN es:

> El método reduce() aplica una función a un acumulador y a cada valor de una array (de izquierda a derecha) para reducirlo a un único valor

Los argumentos de `reduce` son:

1. **El valor *actual.***
2. **El valor *anterior.***
3. El índice *actual.*
4. El arreglo de la que llamas a `reduce.`

Los dos primeros son requeridos y los que usarás mas a menudo.

Un ejemplo común de uso de reduce es obtener un cálculo sobre los datos.

> ¿Cómo obtienes la multiplicación de todos los elementos de arreglo? (hazlo antes de mirar la respuesta y me puedes escribir tu solución).

...

...

...

...

...

…

Hay distintas formas de solucionar este problema

Pero con `Array.reduce` la solución es inmutable!

```javascript
// Solución 3: Inmutable y Declarativa

const res3 = array.reduce((acumulador, actual) => {
  return acumulador * actual 
}, 1)

console.log({ res3 })
```

El flujo de esta solución es el siguiente:

- Declaramos nuestra función reductora y el valor inicial para `acumulador` que es `1`.
- Al iniciar la ejecución del método `reduce` y recorrer el arreglo, el valor de la variable `acumulador` es 0.
- La función reductora se ejecuta en cada iteración recibiendo nuevos valores en sus argumentos.
- En cada llamada a nuestra función reductora nosotros tomamos el valor de `acumulador` y lo multiplicamos con el valor de la variable `actual`
- Al terminar las iteraciones `reduce` retorna el valor final de `acumulador`

En resumen. Usarás `reduce` cada vez que necesites hacer transformaciones complejas de un arreglo, transformaciones que incluyan disminuir o "reducir" el arreglo.

#### A tener en cuenta!

- No olvides el `return` !
- No olvides definir el valor inicial (Que puede ser cualquier tipo de valor, incluyendo un objeto)

Si quieres saber más sobre como funciona `reduce` bajo el capó, puedes revisar el [Polyfill que mozilla ofrece](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce#polyfill) que basa su implementación en un ciclo `while`.

Puedes encontrar el código usado en [este artículo en este enlace.](https://jsitor.com/tML0-9FsI)

## ¿Cómo se relaciona con React?

Una aplicación se trata generalmente de desplegar datos en la interfaz, estos datos generalmente provienen desde alguna API y son comúnmente representados como una colección.

**¿Entonces, cual es la relación?** React puede renderizar una lista de componentes React dentro de otro, por lo que poder manipular arreglos para definir la estructura de datos que necesitamos es esencial.

Un rápido ejemplo:

```javascript
/* On React */
function ItemList({ data, owner }) {
  return (
	<ul>
	  {
		data
		  .filter(item => item.owner === owner)
		  .map(item => (
			<li key={item.id} > {item.name}</li>
		  ))
	  }
	</ul>
  )

}
```

# Conclusión

En resumen, manipular arreglos es parte del trabajo diario de un desarrollador Javascript, conocer diferentes métodos para esta tarea te ayudará a hacerlo de mejor y mas eficiente forma.

De todos estos métodos hay al menos 3 que son poderosos y de uso constante.

- `Array.map`
- `Array.filter`
- `Array.reduce`

## Más recursos

Con estos métodos es posible realizar todo tipo de tareas más complejas como:

- [Inicializar un arreglo.](https://egghead.io/lessons/javascript-4-formas-de-llenar-un-arreglo-en-javascript?af=4cexzzz)
- [Actualizar el contenido de un arreglo de objetos.](https://egghead.io/lessons/javascript-3-formas-de-actualizar-un-arreglo-de-objetos-en-javascript?af=4cexzz)
- [Eliminar elementos de un arreglo.](https://egghead.io/lessons/javascript-como-eliminar-elementos-dentro-de-un-arreglo?af=4cexzz)
- [Remover elementos duplicados desde un arreglo.](https://egghead.io/lessons/javascript-4-formas-de-remover-elementos-duplicados-de-un-arreglo-con-javascript?af=4cexzz)

Cada uno de esos enlaces de llevará a una lección gratuita en [egghead.io](https://egghead.io?af=4cexzz)

