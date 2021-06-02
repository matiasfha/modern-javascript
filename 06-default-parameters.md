    # Parámetros por defecto

    Los parámetros por defecto o predeterminados de una función son una forma de permitir que los parámetros o argumentos que la función recibe sean inicializados con una valor predeterminado evitando así que el argumento sea `undefined`

    ¿Por que querrías evitar los `undefined`?

    En Javascript el valor por defecto de los parámetros de una función es `undefined`. Significa que si no pasas un argumento al momento de llamar la función el parámetro será `undefined` dentro de la misma.

    ```jsx
    function holaMundo(mensaje) {
    	console.log(`Hola mundo: ${mensaje}`)
    }
    holaMundo()
    ```

    En este ejemplo, ¿Cuál será el resultado?
    Al llamar la función `holaMundo()` , sin pasar el argumento `mensaje` el resultado será `Hola mundo: undefined` (pero `undefined` no se renderiza en la consola).

    ### ¿Cómo evitarías ese valor `undefined`?

    Digamos que quieres que `mensaje` diga `<vacio>` cuando no se pasa un argumento a la llamada de la función, un patrón muy usado es el de usar condicionales para averiguar el valor del parámetro y decidir que hacer

    ```jsx
    function holaMundo(mensaje) {
    	mensaje = typeof mensaje !== 'undefined' ? mensaje : '<vacio>';
    	console.log(`Hola mundo: ${mensaje}`)
    }
    holaMundo()
    ```

    Puedes ver el [demo aqui](https://jsitor.com/yFfdNTXCC)

    Aquí usamos un condicional "terciario" que básicamente es lo mismo que escribir

    ```jsx
     function holaMundo(mensaje) {
    	if(typeof mensaje === 'undefined'){ 
    		 mensaje = '<vacio>';
    	}
    	console.log(`Hola mundo: ${mensaje}`)
    }
    holaMundo()
    ```

    Pero este patrón tiene ciertas complicaciones o "code smell":

    - Primero: Es bastante verboso. Imagina tener que escribir esos condicionales para cada parámetro de la función.
    - Segundo: Haces una re-definición de un parámetro de una función lo que por lo general se considera un posible bug y mala práctica. Tu función no debería sobre-escribir los valores pasados a ella, incluso si el valor no fue realmente definido.

    ### ¿Cómo hacer esto mismo de una mejor manera?

    Desde ES6 que el patrón "default parameters" se encuentra disponible en el lenguaje. Simplemente debes usar la sintaxis de asignación: `= `

    ```jsx
    function holaMundo(mensaje = '<vacio>') {
    	console.log(`Hola mundo: ${mensaje}`)
    }
    holaMundo() // Hola mundo: <vacio>
    holaMundo(undefined) // Hola mundo: <vacio>
    holaMundo("Micro bytes") // Hola mundo: Micro bytes
    ```

    ## ¿Cómo funciona?

    Cuando no pasamos ningún parámetro a la función `holaMundo()` el argumento `mensaje` toma el valor definido ``<vacio>``. Pero, si pasamos un valor no definido al parámetro, como en el segundo caso, el argumento nuevamente tomará el valor definido ya que lo que se pasó como argumento sigue siendo `undefined`.

    Sólo en el tercer caso, el argumento toma el valor del parámetro utilizado.

    Esta característica otorga algunas otros super poderes a la hora de definir funciones, por ejemplo:

    - **Usar otro parámetro como valor por defecto**

        es posible asignar un parámetro como valor por defecto que referencia a otro parámetro 🤯

        ```jsx
        function sumar(x = 1, y = x, z = y) {
           return x + y + z
        }
        sumar()
        ```

        **¿Cuál será el resultado?**

    - **User una función como parámetro:**

        Dado que las funciones son "valores", estas pueden ser pasadas como parámetro por defecto.

        ```jsx
        function impuestoBase() {
        	return 0.19
        }

        function obtenerPrecio(valor, impuesto = valor * impuestoBase()) {
            console.log(valor, impuesto)
        }
        const precioConIva = obtenerPrecio(100)
        console.log(precioConIva)
        ```

        En la función `obtenerPrecio` llamamos a la funcion `impuestoBase` que retorna un valor `0.1` y usamos ese valor para calcular el total de impuesto del precio pasado como parámetro.

        **¿Cómo puedes aprovechar esta característica para emitir un error si el parámetro no fue pasado o su valor es undefined?**

        ## ¿Cómo se relaciona con React?

        Es un patrón común que permite diseñar tus componentes con una API (las props) flexible permitiendo al usuario del componente definir sólo lo que necesita. Puedes verlo como props opcionales.

        ```jsx
        const Button = ({ children, variant = 'primary' , rounded = false}) => {
        		const roundedClass = rounded ? `rounded` : ''
        		const variantClass = `button-${variant}`
        		const className = `${variantClass} ${roundedClass}`
        			
        		return <button className={className}>{children}</button>
        }

        ...
        ...
        <Button>Primary no rounded</Button>
        <Button rounded>Primary and rounded</Button>
        <Button variant="danger">Danger and no rounded</Button>
        ```
