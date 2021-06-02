# ¿Qué es ESModules?

Son la definición del estándar ECMAScript para trabajar con módulos. Hasta ahora  nodeJS utilizaba la especificación CommonJS desde hace mucho tiempo pero el browser nunca tuvo un sistema de módulos, hasta la llegada de esta estandarización.

Esta estandarización fue completada con la publicación de ES6 (EcmaScript 2015) pero los navegadores se “tomaron un tiempo” en implementar esta característica, pero al fin ES Modules son soportados por Chrome, Safari, Edge y Firefox.

Utilizar módulos es genial, antes de esta especificación hacíamos extraños “hacks” para crear módulos

> Un ejemplo de una implementación de módulos antes de la llegada de ESModules es el uso de IIFE, método que se basa en el uso de closures. Puedes leer más [en este artículo en FreeCodeCamp](https://www.freecodecamp.org/espanol/news/que-es-un-closure-en-javascript/)

Un módulo es una forma de encapsulación, te permite “encerrar” funcionalidades y exponer una API definida para ser utilizada en otras áreas de tu aplicación. Como una librería.

## ¿Cómo trabajar con módulos?

Desde hace años que hemos usado módulos pero sólo gracias a la existencia de “bundlers” o empaquetadores como webpack, esto ya que los navegadores no soportaban el uso de módulos, necesitábamos tomar todos los módulos y crear nuevos archivos javascript que fuesen interpretables por el navegador.

La sintaxis es seguramente conocida ya, utilizas la palabra clave `import` para "llamar" el modulo y acceder a su contenido.

Un módulo javascript es un archivo que **exporta** uno o más valores, estos pueden ser: funciones, objetos o variables.

```javascript
const capitalizeFirst = ([first, ...str]) => first.toUpperCase() + str.join('') 
export default capitalizeFirst
```

En este ejemplo hay que asumir que el código está dentro un archivo llamado `capitalizeFirst.js` . Este módulo exporta un sólo valor "por defecto”, listo para ser utilizado en cualquier otro archivo javascript:

```javascript
import capitalizeFirst from './capitalizeFirst'

console.log(capitalizeFirst('esta es la frase'));
```

> En este caso como el módulo exporta solo un valor y por defecto, puede renombrar como gustes el  módulo importado en el `import` utilizado.

Los módulos también pueden exportar múltiples valores, esto es conocido como “named exports” (exportaciones con nombre).

```javascript
export const stringToPastelColour = function (str: string): string {
	let hash = 0;
	for (let i = 0; i < str.length; i++) {
		hash = str.charCodeAt(i) + ((hash << 6) - hash);
		hash = hash & hash; // Convert to 32bit integer
	}
	const shortened = hash % 360;
	return 'hsl(' + shortened + ',100%,35%)';
};

export const hexToRgba = (hex: string, a = 1): string => {
	if (!hex) {
		return '';
	}

	const parts = hex
		.replace(/^#?([a-f\d])([a-f\d])([a-f\d])$/i, (m, r, g, b) => '#' + r + r + g + g + b + b)
		.substring(1)
		.match(/.{2}/g)
		?.map((x) => parseInt(x, 16));

	const [r, g, b] = parts as number[];

	return `rgba(${r}, ${g}, ${b}, ${a})`;
};
```

Este módulo exporta dos funciones: `stringToPastelColour` y `hexToRgba` , ambas funciones pueden ser usadas en cualquier otro archivo javascript (o typescript) simplemente importando sus nombres

```javascript
import { stringToPastelColour } from './utils/colors'
```

## ¿Cómo usar módulos directamente en el browser?

Lo primero es definir un archivo javascript que quieras cargar en tu sitio, por ejemplo usemos el archivo `capitalizeFirst.js`

Luego, en el archivo HTML simplemente usamos el tag `<script>`pero con un nuevo atributo `type`

```javascript
<!-- index.html -->
<script type="module">
	import capitalizeFirst from './capitalizeFirst.js'
	console.log(capitalizeFirst("este es el texto"))
</script>
```

El uso de  `type="module"` es el que hace "la magia”. Este atributo le informa al navegador que el código que queremos usar es un "modulo” y no un simple `script`.

Pero  normalmente no escribimos scripts “in-line” como el anterior, si no que tenemos archivos javascript en donde importamos módulos, ¿Cómo usar estos directamente en el navegador?

```javascript
// script-src.js
import capitalizeFirst from './capitalizeFirst.js'
console.log(capitalizeFirst("este es el texto"))



<!-- index.html -->
<script type="module" src="./script-src.js"></script>
```

Por cierto, el uso de imports dinámicos también es soportado en el navegador. Te invito a revisar este video en egghead.io para más información.

[Importar módulos de JavaScript de forma dinámica en el navegador](https://egghead.io/lessons/javascript-importar-modulos-de-javascript-de-forma-dinamica-en-el-navegador)

## Conclusión

Espero que esta introducción te haya sido útil. Y si te preguntas ¿Cómo se relaciona esto con React?, bueno la respuesta es algo más compleja ya que no usarás directamente la importación de módulos en el navegador si no que utilizas alguna herramienta que “compila” tu código y crea pequeños “chunks” de javascript.
Actualmente algunos “bundlers” como `esbuild` crean javascript moderno para ser usado por el navegador incluyendo el uso de módulos.

