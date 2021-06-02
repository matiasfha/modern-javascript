Comencemos a revisar el primer tópico de este micro-curso: Javascript necesario para React, que tal como comenté en el email de bienvenida es en realidad Javascript escencial para <Elije tu framework>.

# Closures

TLDR;  Tienes una closure cuando una función cualquiera accede a una variable fuera de su contexto.

Un elemento escencial en el desarrollo de software es la existencia y uso de funciones, ¿por qué? Por que es la forma "natural" de encapsular cierta lógica y compartirla con otras partes de tu aplicación.

En Javascript un **closure** es creado cada vez que una función es creado, y dado que este tipo de funciones pueden acceder a valores fuera de su propio contexto, el uso de **closures** permite crear y manejar el concepto de **estado** o variables privadas que pueden ser accesibles incluso  cuando la función padre dejo de existir después de su invocación.

```jsx
function exterior(){
	const mensaje = 'Hola mundo';
  function interior() {
       return mensaje;
  }
  return interior;
}

const foo = exterior();
foo(); //retorna el mensaje "Hola mundo"
```

Aquí tenemos la variable `mensaje` dentro de la función `exterior`. Es una variable local y no puede ser accedida desde fuera de la función, pero si desde el interior de la función en la función `interior`.

Cuando asignamos la función `exterior` a la variable `foo` lo que ocurre es lo siguiente:

- La función `exterior` se ejecuta una vez y `foo` se convierte ahora en la declaración de una función (se le asigna el "valor" de lo que retorna `exterior` que en este caso es `interior`).
- La variable `foo` tiene acceso a la función (closure) `interior` y desde ahí a la variable `mensaje`.

En javascript los closures son creados con toda la información del entorno donde fueron creadas. La función `foo` tiene una referencia al closure `interior`, el que fue creado durante la ejecución de la función `exterior`. La función `interior` (el closure) mantiene la información de su ambiente: La variable `mensaje`.

Puedes leer más sobre Closures en [https://www.freecodecamp.org/espanol/news/que-es-un-closure-en-javascript/](https://www.freecodecamp.org/espanol/news/que-es-un-closure-en-javascript/)

## ¿Por qué aprender sobre Closures para trabajar con React?

Closures están por todas partes en React, sobre todo ahora con la existencia y uso de la API de hooks. Los hooks se basan fuertemente en el uso de closures, lo que permite que los hooks sean tan expresivos y simples.

Un problema que se presenta con este extensivo uso de closures es el de "el closure rancio" (stale closure) ¿WTF es esto?

### Stale Closure

Revisemos el siguiente código

```jsx
function contador(){
  let value = 0;
  function incrementar() {
    value++;
    console.log(value)
  }
  function decrementar() {
    value--;
    console.log(value)
  }
  const mensaje = `El valor actual es ${value}`
  function log(){
    console.log(mensaje)
  }
  return [incrementar, decrementar, log]
}
const [incrementar, decrementar, log] = contador()
incrementar(); // logs 1
incrementar(); // logs 2
decrementar(); // logs 1
// No funciona
log();       // logs "El valor actual es 0"
```

Puedes ver el [demo aqui](https://jsitor.com/UQpQtnE_I)

Claramente algo extraño pasa aquí, la función `log` nos muestra que el valor es `0` cuando sabemos que debe ser `1`.

Esta es un "stale closure". El closure `log` capturó el valor de `mensaje` en la primera ejecución de la función `contador` cuando el valor de `value` era `0`.

### ¿Cómo arreglarlo?

La solución es "sencilla". Primer debemos notar que el closure `log` se cerró sobre la variable `mensaje` que no es la variable que cambia. Entonces, debemos refactorizar este trozo de código para cerrar nuestro closure sobre la variable que realmente cambia

```jsx
...
...
function log() {
	const mensaje = `El valor actual es ${value}`
	console.log(mensaje)
}
...
...
```

### ¿Qué relación tiene con React?

Sabemos que los hooks se basan en el funcionamiento de las closures, y uno de los hooks más "complejos" y utilizados es  ``useEffect``. Este hook se utiliza para ejecutar **efectos,** es decir, para sincronizar el estado interno del componente con algún estado externo. 

> Puedes leer más en

> [https://matiashernandez.dev/react-useeffect-hook-comparado-con-los-estados-del-ciclo-de-vida](https://matiashernandez.dev/react-useeffect-hook-comparado-con-los-estados-del-ciclo-de-vida)

> [https://matiashernandez.dev/react-useeffect-por-que-el-arreglo-de-dependencias-es-importante](https://matiashernandez.dev/react-useeffect-por-que-el-arreglo-de-dependencias-es-importante)

`useEffect` recibe un `callback` que es en efecto un closure y un arreglo de dependencias que le indica que "valores observar" para ejecutar el closure definido.

Si no indicas ningún valor en el arreglo de dependencias el closure usado como primer argumento se "cerrar" sobre los valores existentes en el primer render.

> seguiremos hablando sobre este hook más adelante

Otro caso es con el hook `useState`. Este hook retorna una tupla con el valor del estado y una función que permite actualizar el estado.

```jsx
function Contador() {
  const [count, setCount] = useState(0);

  function onClick() {
    setTimeout(() => {
      setCount(count + 1);
    }, 1000);
  }

  return (
    <div>
      {count}
      <button onClick={onClick}>+1</button>
    </div>
  );
}
```

Este simple componente renderiza un botón que al ser clickeado actualiza el estado añadiendo `1` al valor de `count` pero lo hace después de 1 segundo.

Al hacer un par de clicks en el botón (más rápido que 1 segundo) se esperaría que el valor de  `count` fuese `2` pero es en realidad ``1``.

Puedes ver el [demo aquí](https://codesandbox.io/s/beautiful-sunset-k1bkl?file=/src/App.js)

Llamada a la acción: ¿Qué crees que pasa y como puedes solucionarlo?
