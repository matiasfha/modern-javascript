# Inmutabilidad

Bienvenido a la octava semana del curso **Javascript para React** de **Micro Bytes**. Gracias por ser parte de esto, espero que la información  haya sido de ayuda hasta ahora.

**Hoy revisaremos: ¿Qué es inmutabilidad?**

Javascript es un lenguaje muy (quizá demasiado) flexible y ofrece varias opciones para manipular todo tipo de estructura de datos, algunas de estas opciones son **Mutables** y otros **Inmutables.**

## Pero ¿Qué es inmutabilidad?

Este concepto comenzó  a tener un auge importante en el desarrollo frontend gracias a que la programación funcional comenzó poco a poco a hacerse presente, siendo quizá el culpable la dupla React + Redux.

Una de las características más importantes de los lenguajes funcionales es que sus estructuras de datos son inmutables lo que ayuda a reducir la complejidad del software. Y si algo sabemos por la experiencia es que la complejidad es un “code smell”.

Es común que nuestras aplicaciones javascript incrementen de forma exponencial su complejidad dado el número de requerimientos de esta, el problema con esto es que llegado a un punto, es casi imposible mantener un modelo mental de todo lo que ocurre en la aplicación, y por sobre todo, poder seguir fielmente el flujo de datos o información y sus transformaciones.

El concepto de **Inmutabilidad** es sencillo y poderoso. Un valor inmutable es algo que no puede ser cambiado.

> La verdad es el constante cambio, la mutación oculta los cambios, los cambios se manifiestan en caos, por lo tanto, el sabio abraza la historia. — The Dao of Immutability

> [https://medium.com/javascript-scene/the-dao-of-immutability-9f91a70c88cd#.b1cayh7g7](https://medium.com/javascript-scene/the-dao-of-immutability-9f91a70c88cd#.b1cayh7g7)

¿Entonces que hacer cuando necesitas un objeto o valor idéntico al original pero solo con algunos atributos diferentes?. Hasta ahora lo que hacíamos es mutar la variable original y continuar con nuestras vidas. Pero si consideramos esa variable inmutable entonces tendremos que hacer una copia y manipular la copia en vez del original.

Pero esta técnica siempre genera alarmas: ¿crear un objeto nuevo por cada cambio es costoso? y al respuesta es: **si, ya que se debe instanciar nuevamente en memoria.**

Dada esta situación, numerosas investigaciones se han realizado generando grandes avances en manejo de memoria y técnicas que optimizan las estructuras de datos para evitar el gigantesco consumo de memoria que significa recrear un objeto una y otra vez por un tiempo prolongado.

[En esta charla de David Nolen](https://www.youtube.com/watch?v=mS264h8KGwk) se explica como estas estructuras se implementan y funcionan.

> Inmutabilidad es trabajar con los hechos, dado que una variable inmutable no puede cambiar podemos decir entonces que estamos hablando de los hechos, la fuente de la verdad.

En los lenguajes funcionales la inmutabilidad es algo que se da por sentado, pero en Javascript es algo que debemos "forzar" mediante buenas prácticas, linting o el uso de librerías tipo “helper” que refuercen estas prácticas.

> Por cierto, si no sabes que es linting, escribí [un artículo sobre ello en freecodecamp]([https://www.freecodecamp.org/espanol/news/que-es-linting-y-eslint/](https://www.freecodecamp.org/espanol/news/que-es-linting-y-eslint/))

## ¿Por que la inmutabilidad es importante?

Básicamente por que te permite representar estados de forma correcta y eliminar los "side effects".  Es mucho más fácil seguir la lógica de un software que no modifica sus variables que uno que permite que sus variables cambien a diestra y siniestra.

Un ejemplo de la importancia de la inmutabilidad es el propio React. Un componente React es básicamente una función que en base a un estado (sus props y estado interno) retorna una interfaz.

Para manejar estado interno React ofrece una función (hook) llamado u`seState.` Esta función retorna una tupla [`estado, funcion actualizadora]` es decir, React nos indica que para modificar el estado debemos utilizar la función actualizadora, no podemos ir directamente y m**utar** el estado, debemos solicitar su cambio.

Cuando hacemos uso de la función actualizadora React crea una copia del estado y modifica solo lo que es necesario e**n la copia.**

## Trabajando con objetos inmutables.

Entonces, queremos asegurarnos de que nuestros objetos no sean mutados. SI vamos a hacer uso de un método para actualizar el objeto este método debe retornar un nuevo objeto. En escencia lo que necesitamos es una **función pura.**

Una función pura tiene dos propiedades escenciales:

- El valor que retorna depende **sólo** de los argumentos de entrada. O lo que es lo mismo, el valor de retorno no cambiará si los valores de entrada no cambian.
- No puede modificar nada que este fuera de su scope.

## ¿Qué herramientas nos ofrece Javascript?

En el caso de los objetos javascript nos ofrece a lo menos dos formas para mantener el principio de inmutabilidad.

### Object.assign

`Object.assign` es un método del objeto `Object` que permite crear un nuevo objeto copiando los valores que le son pasados como parámetros.

```other
Object.assign({}, data, { attribute: 'something' }
```

En este ejemplo lo que estamos haciendo es copiar todo el contenido del objeto `data` dentro del objeto vació que pasamos como primer argumento, además copiamos y en efecto sobreescribimos el valor del atributo `attribute` dentro del nuevo objeto. `Object.assign` retorna el valor del nuevo objeto.

### Operador spread

Ya revisamos lo que podemos hacer con el operador spread en una lección anterior. En este caso utilizaremos el operador para efectuar lo mismo que con `Object.assign`

`{...data, attribute: 'something' }`

### Arreglos

El objeto `Array` define algunas operaciones mutables, es decir, modifican el arreglo original al operar sobre el: `push`, `pop`, `splice`, `shift`,`unshift`, `reverse` y `sort`. Usar estos métodos implica la existencia de *side effects* y posibles bugs difíciles de rastrear.

Transformemos estos métodos para funcionar de forma inmutables, usaremos el siguiente arreglo como base en cada operación.

```other
const usuarios = [
	{ 
		id: 1,
		name: "brad gibson",
		email: "brad.gibson@example.com"
	},
	{
		id: 2,
		name: "Kuzey Karaduman",
		email: "kuzey.karaduman@example.com"
	},
	{
		id: 3,
		name: "Heinz-Werner Konrad",
		email: "heinz-werner.konrad@example.com"
	}
]
```

### Push

La forma usual es utilizar

```other
const newUser = {
	id:4,
	name: "Stephen Watkins",
	email: "stephen.watkings@example.com"
}
usuarios.push(newUser) // [{ id: 1}, {id:2}, {id: 3}, { id: 4} ]
```

Pero esto modifica el arreglo inicial, la forma inmutable de esta operación se obtiene al utilizar el operador spread.

```other
const newUsuarios = [...usuarios, newUser ]
```

### Unshift

Este operador es similar a `push`, salvo que en vez de agregar un elemento al final del arreglo lo hace al principio (`prepend`).

```other
čonst newUser = {
	id:4,
	name: "Stephen Watkins",
	email: "stephen.watkings@example.com"
}
usuarios.unshift(newUser) // // [{ id: 4}, {id:1}, {id: 2}, { id: 3} ]
```

Nuevamente podemos hacer uso del operador `spread` para conseguir la versión inmutable

```other
const newUsuarios = [newUser, ...usuarios]
```

### Pop

Este operador remueve el último item del arreglo y lo retorna

```other
const user = usuarios.pop(); // user = { id: 3 } y usuarios ahora tiene sólo dos elementos
```

Para lograr lo mismo de forma inmutable tenemos que hacer uso de otro método de arreglo a modo de "helper" y ejecutar algunos otros pasos:

```other
const user = usuarios[usuarios.length - 1]; //Obtenemos una copia del ultimo usuario para replicar el comportamiento de `pop` de retornar el elemento
const newUsuarios = usuarios.slice(0, usuarios.length - 1)// el arreglo original sigue sin modificarse
```

Aquí utilizamos `slice` que revisaremos más adelante.

### Shift

Este operador actúa de forma similar a pop y es en efecto el acto contrario a `unshift`. Remueve un elemento del principio del arreglo y lo retorna.

```other
const user = usuarios.shift(); // user = {id: 1} y usuarios ahora tiene solo 1 element
```

Nuestra solución inmutable nuevamente debe hacer uso de `slice` para replicar el comportamiento

```other
const user = usuarios[0]; //Obtenemos una copia del primer usuario para replicar el comportamiento de `shift` de retornar el elemento
const newUsuarios = usuarios.slice(1) // el arreglo original sigue sin modificarse
```

### Splice

Este método es utilizado tanto para remover como para agregar elementos en cualquier posición de un arreglo

```other
usuarios.splice(1,2, newUser);
```

Esta operación modifica el arreglo original , removiendo dos elementos desde la posición 1 y agregando en su lugar el `newUser`

el resultado es

```other
{
    id: 1,
    name: "brad gibson",
    email: "brad.gibson@example.com"
  },
	{
		id: 4,
	  name: "Stephen Watkins",
	  email: "stephen.watkings@example.com",
	}
```

Para transformar esta operación en inmutable nuevamente hacemos uso de `splice` y  el operador  `spread`

```other
const newUsuarios = [...usuarios.slice(0,1), newUser, ...usuarios.slice(3)]
```

`slice` obtiene un trozo del arreglo original y luego con `spread` este trozo se expando para copiarse dentro del nuevo arreglo.

### Sort y reverse

Finalmente tenemos los métodos `sort` y `reverse` uno ordena el arreglo y otro lo invierte

Cabe destacara que `sort` puede recibir un parámetro que define como se deben comparar los elementos del arreglo para definir cual va primero.

En nuestro caso estamos utilizando un arreglo de objetos, y para su ordenamiento lo haremos comparando los atributos `id`

```other
function compare(a, b) {
  if(a.id > b.id) {
    return -1
  }
	if(a.id < b.id) {
		return 1
	}
  return 0
}
```

Esta función indica que si el elemento `a` tiene un `id` mayor que el del element `b` entonces retorna un valor menor que 0 que significa que `a` debe posicionarse primero. En caso contrario se retorna un valor mayor que 0, entonces `b` debe posicionarse primero. Si retorna 0, entonces los elementos se dejan en la misma posición.

Ahora con esto en su lugar podemos ordenar e invertir nuestro arreglo

```other
usuarios.sort(compare); // [{ id: 3}, {id: 2}, {id: 1}]
usuarios.revserse(); // [{ id: 1}, {id: 2}, {id: 3}]
```

Estos operadores son mutables por defecto y no tienen una contrapartida en el mundo de la inmutabilidad, por lo que para replicar su comportamiento lo que hacemos es la solución más sencilla ([La navaja de Ockham](https://es.wikipedia.org/wiki/Navaja_de_Ockham)), es decir, obtenemos una copia del arreglo y operamos sobre esta copia.

```other
const sorted = [...usuarios].sort(compare)
const inverted = [...usuarios].reverse()
const sortedAndInverted = [...sorted].reverse()
```

Como siempre te dejo el demo de [todo este código aquí](https://jsitor.com/nl0cMEizV)

#### Librerías

Existe también la opción de utilizar alguna librería que te permita aplicar los principios de inmutabilidad de una forma mas directa. Este tipo de librerías garantizan que el principio está siendo aplicado.

Una de estas librerías y quizá la más mencionada es [Immutable.js](https://immutable-js.github.io/immutable-js/) que tuvo gran auge al ser usada junto con Redux.

## Conclusión

La inmutabilidad no es un tópico específico de Javascript, puede ser aplicado a cualquier lenguaje y es muy recomendado que así sea. Lo importante aquí es entender por que requieres utilizar inmutabilidad y como estás manejando tus datos para mantener un flujo de datos legible y un patrón de código mantenible.

