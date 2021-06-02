    # Nullish Coalescing y Optional Chaining

    En esta ocasión revisaremos dos características recientemente añadidas a javascript - que han sido soportadas por [Babel](https://babeljs.io) desde hace mucho - 

    > Por cierto, puedes aprender más sobre ¿Qué es Babel y cómo funciona? en [este artículo en FreeCodeCamp en español](https://www.freecodecamp.org/espanol/news/que-es-babel/)

    Cada año Javascript recibe actualizaciones basadas en lo que el equipo del TC39 define para la especificación ECMA. Las características que hoy revisamos forman parte de la versión ES2020.

    > En otra nota más, [en este episodio de Café con Tech](https://www.cafecon.tech/1081172/3872285-que-es-es6) te cuento que es la especificación Ecma y el TC39

    Puedes encontrar la documentación y propuesta de ambas características en los siguientes enlaces

    - Nullish Coalescing: [Documentación](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator) y [Propuesta](https://github.com/tc39/proposal-nullish-coalescing)
    - Optional Chaining: [Documentación](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining) y [Propuesta](https://github.com/tc39/proposal-optional-chaining)

    ## Nullish Coalescing (operador fusión nula)

    El operador fusión nula es un operador lógico que retorna el valor utilizado en el lado derecho de la expresión cuando el lado izquierdo es `null` o `undefined` en caso contrario, retorna el lado izquierdo. ¿Qué?. Básicamente, es un operador que evalua si el valor existe o no y define un valor de retorno.

    ```jsx
    const foo = null ?? 'mensaje por defecto'
    console.log(foo) // 'mensaje por defecto'
    ```

    Quizá esta sintaxis te recuerde un patrón bastante usado en javascript, el uso del operador lógico OR `||`. La gran diferencia está en que este operador retorna el lado derecho de la operación cuando el lado izquierdo es un valor `falsy` (ya revisamos que valores eran `falsy` en JS en el tema anterior). Este operador es usualmente utilizado para definir valores por defecto (aunque ya sabemos que podemos utilizar otro formato para lograrlo en el caso de funciones).

    Veamos un ejemplo:

    ```jsx
    const data = {
      configuration: {
        nullValue: null,
        width: 400,
        time: 0,
        text: '',
        isOpen: false
      }
    };

    const undefinedValue = data.configuration.undefinedValue ?? 'un valor por defecto'; // un valor por defecto
    const nullValue = data.configuration.nullValue ?? 'un valor por defecto'; // un valor por defecto
    const text = data.configuration.text ?? 'un valor por defecto'; // ''
    const time = data.configuration.time ?? 10; // 10
    const isOpen = data.configuration.isOpen ?? true; // false
    ```

    Como siempre, puedes [ver el demo en este enlace](https://jsitor.com/od_YGsHEj-). y comparar el funcionamiento del operador `??` versus el operador `||` (operador OR)

    También te invito a ver un video sobre esta característica en grabada para [egghead.io](http://egghead.io) [https://egghead.io/lessons/javascript-operador-fusion-nula-nullish-coalescing-en-javascript](https://egghead.io/lessons/javascript-operador-fusion-nula-nullish-coalescing-en-javascript) 

    ## Optional Chaining

    Esta es una característica presente en muchos otros lenguajes cuya adición ha sido pedida desde hace mucho tiempo y en la iteración ES2020 el operador llegó a javascript para quedarse.
    A la fecha de escritura de este artículo el operador está soportado en todos los browsers actuales y en caso de necesitarlo siempre puedes usar Babel para dar soporte a navegadores más antiguos.

    Este operador viene a resolver un caso de uso muy común a la hora de obtener datos desde una fuente externa. Normalmente estos datos son obtenidos en formato JSON creando un objeto de datos anidados. Uno de los problemas es cuando no conocemos la "forma" de esos objetos y cuando estos objetos no tienen una declaración definida de que atributos están o no presentes en el mismo.

    ```jsx
    const user = {
    	profile: {
    		firstName: 'Primer nombre',
    		lastName: 'Apellido',
    	},
    	avatar: 'alguna url'
    }
    console.log(user.firstName) // undefined
    console.log(user.personalData.email)  // Error!
    ```

    En este ejemplo tenemos un objeto de usuario al que queremos acceder. En el primer caso se intenta acceder a un atributo inexistente por lo que el resultado es `undefined`, pero el segundo caso es problemático. Intentamos acceder al atributo `personalData` que es `undefined` y después de eso se accede al atributo ``email``, pero dato que `personalData` no existe el engine nos muestra un Error
    `TypeError: undefined is not an object (evaluating 'user.personalData.email') at line 46 col 30`

    La solución a esto hasta antes de la existencia del operador de encadenamiento opcional (optional chaining) era algo similar a esto

    ```jsx
    const email = user && user.personalData && user.personalData.email
    console.log(email); // undefined
    ```

    Pero ahora lo que podemos hacer de forma nativa e idiomática es mucho más expresivo

    ```jsx
    const email = user?.personalData?.email
    console.log(email)//
    ```

    El operador `?.` evita el error que causa el operador de encadenamiento `.`. Si una referencia es nula (`null` or `undefined`) la expresión se detiene (short-circuit) retornando `undefined`.
    Incluso el operador puede ser utilizado en funciones retornando `undefined` si la función no existe.

    > El operador no puede utilizarse en variables que no han sido declaradas

    ```jsx
    console.log(user.profile.getEmail?.()) // undefined
    console.log(superUser?.profile.firstName) // Error `superUser` no existe
    ```

    > Notar que el operador completo es `?.`

    La sintaxis descrita para este operador es

    ```jsx
    obj.val?.prop
    obj.val?.[expr]
    obj.arr?.[index]
    obj.func?.(args)
    ```

    Es decir puede ser utilizado también con arreglos para acceder de forma segura a alguno de sus index.

    Como siempre, puedes ver el [demo de todo este código en este enlace](https://jsitor.com/od_YGsHEj-)

    Y también puedes verlo en video [en este material grabado para egghead.io](https://egghead.io/lessons/javascript-encadenamiento-opcional-en-javascript)

    > Cuéntame respondiendo a este mismo correo que papel crees que juegan estos operadores al ser utilizados con React.
