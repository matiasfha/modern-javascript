¿Qué son esas extrañas comillas ``  en Javascript? Son cómo los strings de toda la vida pero con super poderes.

Fueron introducidas el año 2015 en la versión conocida como ES6. La idea es otorgar nuevas formas de declarar strings (cadenas de texto) a la vez que adopta algunas construcciones que ya tenían cierta popularidad.

La sintaxis es sencilla y muy similar a un string normal.

```jsx
const str = `un template literal`
```

Pero tiene algunas características que un string no posee

- Permite definir strings multi linea.
- Provee una forma sencilla de interpolar variables y expresiones dentro del string.
- Permite crear tu propio DSL (Domain Specific Language) utilizando **template tags**. Este es el caso de la api de styled-components.

## Strings multilinea

Utilizando strings, la única forma de crear un string de multiples lineas era utilizando el caracter `\` al final de cada línea.

```jsx
const multilinea = 'este string \
tiene multiples \
lineas';
```

Esto permite crear el string en múltiples lineas en el código pero el resultado al renderizar sigue siendo una sola línea.

Si lo que buscabas es renderizar en múltiples líneas, entonces es necesario utilizar el carácter "salto de linea"

```jsx
const multilinea = 'este string \n \
tiene multiples \n \
lineas';
const multiple = 'este string\n tiene multiples\n lineas';
```

Utilizando template literales esto se hace más sencillo, un string multi linea definido de esta manera se renderiza tal como lo escribiste 😄

```jsx
const multilinea = `este string
tiene multiples
lineas`;
```

Puedes ver un [demo aquí](https://jsitor.com/-wUZUq8mq)

## Interpolación

La interpolación es el proceso de desplegar el valor de una variable o expresión dentro de un string. Utilizando template literales esto se logra de manera "directa" sin tener que estar abriendo y cerrando el string para agregar el valor a renderizar.

```jsx
const foo = 'Alguna variable';
const str = `foo = "${foo}"`; // foo = "Alguna variable"
```

Dentro de las llaves `${}` puedes agregar cualquier valor o expresión que quieras, el engine de javascript se encargará de evaluar y desplegar

```jsx
const miFuncion = () => {
 // hace muchos calculos
 return 'miFuncion'
}
const str = `miFuncion ${miFuncion()}`
const str = `${ miFuncion() === 'miFuncion' ? 'Funciono!' : 'No funciono'}`
/* esto es lo mismo que 
let str = 'No funciono'
if(miFuncion() === 'miFuncion') {
    str = 'Funciono'
}
*/
```

## Template tags

Tagged templates es la característica que permite que exista la api de [styled-components](https://styled-components.com) o [Apollo graphql](https://www.apollographql.com/docs/react/data/queries/#executing-a-query). Quizá no suene muy interesante al principio, no todos escribimos nuestro propio DSL para nuestro día a día, pero es importante entender como funciona.

En el caso de styled-components, los template tags son utilizados para definir componentes y poder escribir css "normal" dentro de javascript

```jsx
const Button = styled.button`
  font-size: 1.5em;
  background-color: black;
  color: white;
`
```

aquí `styled.button` es en efecto una función que acepta como argumento la plantilla literal (template literals) y retorna un componente.

Esto permite que, en vez de que el browser asigne inmediatamente un valor a la variable, podamos manipular el contenido de este template literal.

La forma en que esto funciona (simplificado) es, primero construyendo una función que se ejecuta sobre el string:

```jsx
function generateTemplate() {
 // hace algo
}
const miTemplate = generateTemplate`<article>
<h1>${nombre}</h1>
</article>`;
console.log(mitemplate);
/*
<article>
<h1>Algún nombre</h1>
</article>
*/
```

Aquí el "tag" o etiqueta es el nombre de nuestra función, el browser ejecutará esta función y pasará como argumento toda la información del string  que pasamos como template literal (``) incluyendo las variables/interpolaciones definidas y lo que sea que se retorne de esta función es lo que se asignará a `miTemplate`.

Es decir, nuestra función trabaja como un paso intermedio entregándonos la posibilidad de manipular la información.

```jsx
function generateTemplate() {
    return 'Este será mi template'
}
```

Si hacemos algo así, el valor de `miTemplate` será el string que retornamos.

Obviamente no es lo que queremos, este función `tag` acepta algunos argumentos que la hacen más interesante:

```jsx
function generateTemplate(strings,...values) {
 // hace algo
}
```

El primer argumento es un arreglo con todos los strings pasados al template literal, en este caso

```jsx
strings = ['<article><h1>','</h1></article>'];
```

y el segundo argumento representa a todas las variables o interpolaciones pasadas al template literal, en realidad aquí utilizamos el operador "spread" para evitar tener que escribir uno a uno los argumentos, es así que `values` es también un arreglo que tendrá los valores de la interpolación.

```jsx
const nombre = "Mi gran artículo";
const miTemplate = generateTemplate`<article>
<h1>${nombre}</h1>
</article>`;

values = ['Mi gran artículo'];
```

Entonces, el punto de usar template tags es que podemos crear nuestro propio string utilizando estos datos.

Un caso de uso bastante usado es el de generar tu propio "syntax highlighter", por ejemplo

```jsx
const highlight = (strings, ...values) => {
     let str = '';
   strings.forEach((string, i) => {
       str += `${string} <span class='hl'>${values[i] || ''}</span>`;
   });
   return str;
}
<style>
.hl {
background: #ffc600
}
</style>
```

Lo que este template tag hace es agregar `<span class='hl'>` a cada uno de los valores que le pasamos a la función, es decir, convierte esto

```jsx
const valor1 = 'sintaxis';
const valor2 = 'iluminada';
const str = highlight`
Mi ${valor1} esta ${valor2}
`
```

En esto

```jsx
const str = "Mi <span class='hl'>sintaxis</span> esta <span class='hl'>iluminada</span>".
```

[Demo](https://jsitor.com/KZYhESWUc)

Llamada a la acción: Puedes escribir una función para template tags que replique la interpolación de valores y expresiones. Puedes usar [este ejemplo como plantilla](https://jsitor.com/eLPF1YwpU).
