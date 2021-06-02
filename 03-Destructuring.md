 # Destructuring

    Destructuring es quizá una de las mas grandes características agregadas a ES6 (2015) y ampliamente utilizada hoy en día, por lo que comprender que es y poder leerlo fácilmente en el código se convierte en algo necesario en tu día a día.

    ## ¿Qué significa destructuring?

    Es una expresión que te permite extraer o "destructurar" datos desde estructuras de datos como arreglos, objetos, mapas y sets y crear nuevas variables con ese dato en particular.

    Te permite extraer propiedades de un objeto, items de un arreglo de una manera "sencilla" y de una sola vez.

    Pensemos en una estructura de datos común y una tarea repetitiva

    ```jsx
    const usuario = {
        firstName: 'Matias',
        lastName: 'Hernández',
        country: 'Chile',
        twitter: '@matiasfha'
    }
    const firstName = usuario.firstName
    const lastName = usuario.lastName
    ```

    Es una tarea común, obtener alguna propiedad de un objeto para ser utilizada dentro de otro bloque lógico, puedes asignarla a una variable como en este caso, o escribir `usuario.firstName` varias veces más adelante.

    Lo que **destructuring** permite, es hacer este proceso de una manera más directa

    ```jsx
    const { firstName, lastName } = usuario
    ```

    Esta es la sintaxis de destructuración de un objeto, usamos `{}` al lado izquierdo de una asignación para indicar que estamos des-tructurando el valor que está al lado derecho.

    Este código simplemente dice: Crea dos variables: `firstName` y `lastName` y toma desde el objeto usuario las propiedades de igual nombre y asigna sus valores.

    Genial no?. Ese es sólo un nivel, esto puede ser utilizando de forma anidada tantas veces como desees

    ```jsx
    const usuario = {
        firstName: 'Matias',
        lastName: 'Hernández',
        links: {
            social: {
                twitter: '@matiasfha'
            }
         }
    }
    const  { twitter } = usuario.links.social 
    // o tambien se puede
    const { links: { social: { twitter } } } = usuario
    ```

    Puedes destructurar en tanto niveles como gustes, pero intenta siempre mantener la legibilidad.

    Pero esta sintaxis no está limitada solo a objetos, también podemos destructurar arreglos.

    ```jsx
    const arreglo = ['Hola','soy','Matias']
    ```

    Digamos que este arreglo es igual en muchas ocasiones y que queremos extraer el saludo y el nombre. ¿Cómo lo harías?
    ...

    ..

    ..

    La forma sin destructuración sería

    ```jsx
    const saludo = arreglo[0]
    const nombre = arreglo[2]
    ```

    Pero si usamos destruturación podemos nombre las variables de forma directa

    ```jsx
    const [saludo, , nombre ] = arreglo
    ```

    ¿Viste lo que hice ahí?. Puedes ver este pequeño [demo aquí](https://jsitor.com/nCuVEFhut)

    Al destructurar un arreglo hacemos uso de la posición del elemento en el arreglo para las asignaciones, así `saludo` es equivalente a `arreglo[0]` . Esto también permite "saltarse" algún item del arreglo que no nos interesa, simplemente escribimos un "fantasma". En vez de escribir el nombre de la variable que no nos interesa, en este caso la posición central o `arreglo[1]`simplemente escribimos las `,`(coma) correspondiente.

    ¿De que forma puedes obtener sólo el nombre utilizando destructuración? Envíame el link de tu solución 😄

    Veamos un ejemplo mas "interesante"

    ```jsx
    function doSomeCalc({x, y, z = 10}( {
        return Math.floor((x + y + z ) / 3)
    }
    // que es lo mismo que
    function doSomCalc(obj) {
        const { x, y, z = 10 } = obj
        return Math.floor((x + y + z ) / 3)
    }
    ```

    Este artículo [en MDN](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Operadores/Destructuring_assignment) te puede entregar más información. Una vez lo revises, te propongo refactorices el siguiente código utilizando desctructuring.

    ```jsx
    function doSomeDestructuring(){
      const data = {
        title: 'Wanda Vision',
        protagonistss: [{
          name: 'Wanda Maximof',
          enemies: [
             {name: 'Grim Reaper'},
             {name: 'Nightmare' },
             {name: 'Agatha Harkness'},
             {name: 'Zelena', title: 'The Wicked Witch'},
    	    ],
    	  },{
          name: 'Vision',
          enemies: [
              { name: 'Doctor Doom' },
              { name: 'Ultron' }
          ]
        }],
      }
      // const {} = data // <-- Reemplaza el uso de múltiples const con destructuring
      const title = data.title
      const protagonistName = data.protagonists[0].name
      const enemy = data.protagonists[0].enemies[3]
      const enemyName = enemy.name
      return `${title}: La protagonista ${protagnostName} lucha contra ${enemyName}`
    }
    ```
