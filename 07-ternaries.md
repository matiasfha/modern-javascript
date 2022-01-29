# El operador ternario

Javascript es un lenguaje muy flexible y existen varias formas de expresar una misma idea, ese es también el caso del operador ternario -  Es una forma alternativa de expresar una condición -

El operador condicional ternario no es exclusivo de javascript, su implementación es parte de la sintaxis básica para expresar condiciones, es comúnmente conocido como "inline if" o simplemente "ternary".

Este es el único operador de Javascript que utiliza 3 operandos: Una condición, un signo `?` y un signo `:`

```jsx
condicion ? expresion1 : expresion2
```

Esto quiere decir que en base a la condición:

1. La `expresion1` se ejecuta si la condición tiene un valor verdadero
2. La `expresion2` se ejecuta si la condición tiene un valor falso.

## Truthy y Falsy

Es buen momento para revisar que es lo que Javascript describe como un valor verdadero (`truthy`) y uno falso (`falsy`). Este es un tema fundamental en el uso del lenguaje  y que debe tener su propio espacio en este curso.

En palabras sencillas y directas:

- **truthy** es un valor que se considera verdadero (true)
- **falsy** es un valor que se considera false (false)

En Javascript existen sólo 6 valores `falsy`: `undefined`, ``null``, `NaN`, `0`, "" (string vacio) y `false`. Por lo que la mejor forma de saber si un valor es `truty` es revisando que no sea `falsy`.

> Como ya sabes Javascript es muchas veces extraño. Una característica del lenguaje es que puedes "envolver" cualquier primitiva en un objeto y dado que un objeto es `truthy` entonces la primitiva será considerada verdadera un ejemplo de eso es `new Number(0)`  o `new String("")` que serán verdaderos. 
¿Qué ocurre con el caso `new Boolean(false)`?

**¿Por qué debería importarme estas rarezas de javascript?** Por dos simples razones:

- Javascrip es el lenguaje que estás escribiendo y concer sus fundamentos y "tricks" te permitirá ser más eficiente a la hora de utilizarlo y
- El que un valor sea truty or falsy indicará la forma en que puedes usarlo dentro de una condición o expresión lógica.

## Tipos de comparaciones

En javascript existen dos tipos de comparaciones "Loosly" y "strict"

El comparador "loosly" o "débil" es cuando comparas utilizando `==` en este caso javascript ejecuta una coerción del valor buscando un tipado común entre ellos y luego comparando la igualdad de sus valores.

Esto genera casos muy extraños como por ejemplo:

```jsx
// Verdaderos
false == 0;
0 == '';
null == undefined;
[] == false;
!![0] == true;

// Falsos
false == null;
NaN == NaN;
Infinity == true;
[] == true;
[0] == true;
```

El comparador `strict` , es decir `===` (triple signo igual) es más seguro ya que primero compara los tipos de los operandos y si estos no son iguales entonces retorna inmediatamente indicado `false`

Utilizando el mismo ejemplo anterior

```jsx

false === 0; // false
0 === ''; // false
null === undefined; //false
[] === false; // false
!![0] === true; // true

// Falsos
false === null; 
NaN === NaN;
Infinity === true;
[] === true;
[0] === true;
```

Puedes ver un demo de [estos comparadores aqu](https://jsitor.com/vsDMmiinh)í

# Como usar el operador ternario

Volviendo al operador ternario. Utilizarlo es bastante sencillo, lo que es importante es decidir cuando utilizarlo con la intención de mantener nuestro código mantenible y legible.

```jsx
let edad = prompt('¿Que edad tienes? :');

// check the condition
let result = (edad >= 18) ? 'adulto' : 'menor';

console.log(`Eres ${result}.`);
```

en este ejemplo leer el operador ternario se hace bastante sencillo.

Si `edad` es mayor o igual que `18` entonces (`?`) retorna `adulto` en caso contrario retorna `menor`.

Puedes agregar cualquier expresión al operador ternario, incluso otro ternario dentro del mismo!! 🤯

Otro ejemplo es el de ejecutar varias acciones a la vez en cada una de las expresiones

```jsx
const authenticated = getAuthStatus();
const nextURL = authenticated ? (
    alert('Estás autenticado'),
        '/admin'
) : (
    alert('Accesso denegado'),
        '/403'
);
// redirect function
console.log(nextURL); // '/admin'
```

Aquí cada acción que se quiere ejecutar esta separada por una coma `,`

## ¿Y donde entra React?

Es un patrón bastante usado cuando se quiere renderizar de forma condicional

Si quieres renderzar una lista pero solo cuando los datos están disponibles ( o la lista tiene un largo mayor a 0) puedes hacer algo como esto:

```jsx
function Lista({datos}) {
    if(!datos.length) {
        return "Cargando..."
    }
    return (
    <React.Fragment>
        <ul>
          {datos.map(item => (
            <li key={item.id}>
              <span>{item.name}</span>
            </li>
          ))}
        </ul>
    </React.Fragment>
  )
}
```

O de forma más sucinta utilizando el operador ternario

```jsx
function Lista({datos}) {

    return (
    <React.Fragment>
            {datos.length ?  (
            <ul>
              {datos.map(item => (
            <li key={item.id}>
              <span>{item.name}</span>
            </li>
          ))}
        </ul>
            ) : 'Cargando...' }
    </React.Fragment>
  )
}
```

- ¿Has utilizado el operador ternario? ¿En que situaciones sueles utilizarlo o donde lo utilizarías?
