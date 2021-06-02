    # ¿Qué son esos 3 puntos (...)?

    Se trata de un "nuevo" (en realidad no tan nuevo ya que es parte de ES6) operador llamado "spread" u operador de propagación, lo que hace es exactamente lo que su nombre dice. 

    Propaga cualquier valor que sea iterable (arreglos o strings) entre 0 o más elementos o argumentos y en el caso de objetos, expande los pares clave-valor de éste

    ¿Cómo se ve esto?

    ```jsx
    const arreglo = [1,2,3,4]
    const newArreglo = [...arreglo] // [1,2,3,4]
    ```

    En este este pequeño ejemplo, usamos el operador propagación (o spread que es más corto) para expandir/propagar el arreglo original dentro de un nuevo arreglo, lo que es lo mismo que clonar el arreglo, creando así dos instancias idénticas en contenido.

    ¿Y para que me sirve hacer esto?

    En escencia lo que este operador hace es escribir `1,2,3,4` dentro de los brackets lo que te permite agregar nuevos elementos.

    ```jsx
    const arreglo = [1,2,3,4]
    const newArreglo = [...arreglo, 5] // [1,2,3,4,5]
    const otroArreglo = [0,...arreglo] // [0,1,2,3,4]
    ```

    ¿Y en el caso de los objetos?

    En el caso de los objetos, el operador spread se usa para expadir y finalmente clonar el objeto original

    ```jsx
    const user = {
    	username: 'Matias',
    	webpage: 'https://matiashernandez.dev',
    	twitter: '@matiasfha'
    }
    const userConExtraData = {
    	...user,
    	extra: {
    		telefono: '+56911111111'		
    	}
    }
    ```

    lo que en efecto es lo mismo que

    ```jsx
    const userConExtraData = Object.assign({}, user, { extra: { 		telefono: '+56911111111'		}})
    ```

    Puedes ver el [demo aquí](https://jsitor.com/aJ9VgO_VG)

    También este operador permite sobre-escribir atributos de un objeto permitiendo así crear un nuevo objeto en base a un original

    ```jsx
    const newUser = {
    	...user,
    	username: 'Otro usuario',
      twitter: '@otroUser'
    }
    /*
    {
    	username: 'Otro usuario',
    	webpage: 'https://matiashernandez.dev',
    	twitter: '@otroUser'
    }
    */
    ```

    Lo que aquí ocurre es el que el operador `...` toma todas las propiedades de nuestro objeto original `user` y las **aplica** al nuevo objeto creado al escribir las llaves, luego nosotros sobre-escribimos estos atributos al escribir explicitamente el valor de una de ellas. El código anterior es igual a:

    ```jsx
    const newUser = {
    	username: 'Matias',
    	webpage: 'https://matiashernandez.dev',
    	twitter: '@matiasfha'
    	username: 'Otro usuario',
      twitter: '@otroUser'
    }
    ```

En el caso de los **strings** podemos usar el modelo mental de que un **string** es una colección o arreglo de caracteres, por lo que al utilizar `...` lo que hacemos es expandir ese arreglo

```jsx
const str = 'String';
console.log(...str); // 'S t r i n g'
```

O lo que es lo mismo

```jsx
console.log(str.split(''))
```

Desafío

¿Qué pasaría si tomas un arreglo y lo "expandes" o "propagas" dentro de un objeto?

Otro lugar en donde se puede utilizar este operador es para aplicar multiples argumentos en funciones.

Ya vimos que al aplicar el operador `...` a un arreglo este se "divide" entre sus elementos entonces, podemos usar este mismo principio para ser bien vagos/flojos y evitar tener que escribir nuestros argumentos.

```jsx
function fn(a, b, c, d) {
  console.log(`${a}, ${b}, ${c}, ${d}`)
}
const a = 'Este es el argumento A'
const args = [a, 'b', 'c', 'd']
fn(...args)
```

Básicamente lo que hacemos es aplicar los argumentos al expandir el arreglo args. Cada elemento del arreglo tomará una posición de entre los argumentos.

**Take home**

¿Crees que funcionará si en vez de usar un arreglo de argumentos usamos un objeto?

## ¿Y que tiene que ver eso con React?

Hay un patrón bastante utilizado en React para evitar tener que escribir todas las props (o argumentos) de un componente

Por ejemplo

```jsx
const MyDummyComponent = (color, height, size, length, text, children) => {
	return (
		... Algo de JSX
	)
}
...
...
<MyDummyComponent 
	color={color}
	height={height}
	size={size}
	length={length}
	text={text}>
		Some children
</MyDummyComponent>
```

Es claro que escribir eso es díficil de matener y muy tedioso. Pero podemos usar el operador `spread` para evitar esto

```jsx
const props = {
	color: 'red',
	height: 100,
	size: 2,
	length: 3,
	text: 'Text',
	children: <p>Otro componente o elemento</p>
}
<MyDummyComponente {...props} />
```

## Resumen

En resumen el operador `...` o `spread` es una forma sencilla y sobre todo fácil de leer para expandir y así reutilizar los elementos de un arreglo, string o las propiedades de un objeto para así crear nuevos datos o estructuras.
Es una forma rápida de "limpiar" tu código para cuando requieras generar datos dinámicos.
**Take Home**

¿En qué otro caso utilizarías o has utilizado este operador?
