Continuando con el operador `...` hoy revisamos que la sintaxis `rest`  que se basa en el mismo principio que el operador `spread`

Pero antes de entrar a probar esta característica ¿Qué problema existe que se necesita una nueva característica?

Imagina una función que puede recibir un número desconocido de argumentos, antes de que esta sintaxis estuviera disponible esta es la forma en que se implementa una solución

```jsx
function multiplesArgumentos() {
	console.log(arguments)
}

multiplesArgumentos(1,2,3); //[1,2,3]
multiplesArgumentos(1,"algo","otro str",4); //[1,"algo","otro str",4]
```

Como siempre, puedes ver el demo para esta [lección aquí](https://jsitor.com/PD4-qoxy0)

Este ejemplo hace uso del objeto ``arguments` este es un objeto local a una función provisto por el engine de Javascript, es un objeto similar a Array que siempre está disponible desde dentro de una función y hace referencia a los argumentos pasados a esa función. `arguments` es similar a `Array` ya que comparten algunos atributos como length y propiedades indexadas, pero no tiene otros métodos de arreglo como `reduce` o `forEach`. Si necesitas hacer uso de un método de arreglo, entonces necesitas transformar el objeto `arguments` a un arreglo usando `Array.from` 

Puedes leer más sobre este objeto en [MDN](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Funciones/arguments)

Un uso común de este objeto es para concatenar cadenas de texto o incluso ejecutar multiples funciones/callbacks.

Este método funciona, y funciona bien pero es medianamente complejo y depende de un objeto fantasma, el objeto `arguments`. La sintaxis rest ofrece una solución mas legible y mantenible.

Utilicemos el mismo ejemplo anterior pero utilizando la nueva sintaxis.

```jsx
function multiplesArgumentos(...args) {
	console.log(args)
}

multiplesArgumentos(1,2,3); //[1,2,3]
multiplesArgumentos(1,"algo","otro str",4); //[1,"algo","otro str",4]
```

Es muy similar, pero, el argumento `args` es un arreglo con todos sus métodos y al mismo tiempo es **declarado claramente** que esta función recibe parámetros, algo que no era claro en la versión anterior.

Puedes ver el mismo demo pero con la nueva [sintaxis aquí](https://jsitor.com/oU6qO2YL1)

Usar el parámetro rest `...` que es la misma sintaxis del operador `spread` te permite pasar un número indefinido de parámetros a una función declarando claramente el nombre del parámetro, incluso puedes pasar parámetros con nombre y un último parámetro tipo ``rest``.

```jsx
function multiplesArguments(primer, ...rest) {
	console.log(primer)
	console.log(rest)
}
```

Este ejemplo, también disponible en el demo puedes ver que los argumento están divididos, uno es el argumento declarado llamado `primer` y el ultimo un arreglo indefinido.

También puedes usar un objeto como parámetro cuyos atributos son indefinidos

```jsx
function multiplesArguments({ primer, ...rest }) {
	console.log(primer)
	console.log(rest)
}
```

### ¿Y dónde entra React aqui?

Los parámetros rest son un patrón común en el desarrollo de aplicaciones React, permitiendo que un componente reciba multiples props que no necesariamente debes declarar o que quizás son heredadas de un componente padre.

```jsx
function Box({className, ...props}) {
  const baseProps = {
    className: `box ${className}`,
    children: 'Empty box',
  }
  return <div {...baseProps} {...props} />
}
```

## **Summary**

Puedes pensar  en la sintaxis `...` como una sintaxis inherente a las colecciones de datos. Esta sintaxis/operador se ejecuta sobre colecciones de valores. El significado de los 3 puntos `...` puede variar dependiendo del contexto en que se use por lo que aprender sus matices es importante para usarla correctamente.

En el caso expuesto en esta lección la sintaxis es utilizada para demostrar el uso de un número indefinido de argumentos en una función permitiendo acceder a ellos como si de un arreglo se tratase.
