# Bit
La tercera práctica se inicia con la función más elemental para la creación de registros en una memoria: guardar un solo bit. El propósito de este chip es precisamente ese, almacenar el valor de un bit de entrada "in" en la salida "out". Además, el dato almacenado en "out" solo cambia cuando el reloj (clock) predefinido del sistema alcanza un nivel alto, como se muestra en la figura a continuación.

<img src="/Images/clock.png">

Para lograr esto, utilizamos dos chips: un multiplexor (Mux) encargado de decidir si se debe o no guardar el dato, y un flip-flop D (DFF) que lee el dato y lo devuelve en una salida llamada "callback". Esta salida "callback" actúa como una entrada para el Mux, permitiendo que este decida si realizar un cambio basándose en si ha ocurrido un cambio en el DFF y si la entrada "load" es "1".

# Register
El chip Register tiene la función de almacenar una entrada "in[16]" de 16 bits en una salida "out[16]". Para lograrlo, llamamos sucesivamente al chip anterior (Bit) de manera iterativa, de modo que se almacene cada posición uno por uno, un total de 16 veces. Esto se realiza de manera similar al chip "Bit", con la única diferencia siendo la coordinación con el reloj (clock) para garantizar que los cambios en cada posición ocurran en el tiempo alto del clock.

# RAM8
Ingresando al ámbito de las memorias RAM, comenzamos con una memoria simple y pequeña que consta de solo 8 ubicaciones para almacenar datos. Para esta funcionalidad, contamos con una entrada adicional en comparación con los chips anteriores, llamada "address", que indica en qué puerto o ubicación se guardará el dato de "in[16]". Un chip DMux se encarga de seleccionar el espacio al que se enviará el dato de acuerdo con la dirección (address). Posteriormente, tenemos una sucesión de 8 chips "Register", cada uno representando una de las ubicaciones de memoria, y finalmente, un chip "Mux" se encarga de emitir el dato almacenado en la ubicación específica en la que se debe guardar.

# RAM64
La RAM64 opera de manera similar al chip "RAM8", pero en este caso, aumentamos el espacio de memoria a 64 ubicaciones. La entrada "address" tiene un tamaño de 6 bits en este caso, lo que permite seleccionar tanto la "RAM8" en la que se almacenará el dato (los primeros 3 dígitos) como la ubicación dentro de la RAM8 en la que se guardará (los últimos 3 dígitos). El DMux determina en qué ubicación de memoria se guardará el dato y emite el dato correspondiente.

# PC
El chip "PC" tiene la función de guardar, incrementar o reiniciar el valor de una ubicación en la memoria. Para lograr esto, se utiliza una memoria simplificada con una sola ubicación de 16 bits, implementada como un "Register". Para realizar estas operaciones, primero se deben llevar a cabo las operaciones posibles: establecer un vector de ceros para la operación "reset" y un incremento en el dato almacenado en un ciclo previo del reloj para la operación de "incremento". Luego, se elige qué dato se guardará: la entrada normal, el incremento o el reinicio. Finalmente, dos condiciones OR determinan si se actualizará el dato o no, dependiendo de si alguna de las entradas es igual a "1". El dato se ingresa entonces en un registro y se actualiza.

# RAM512
Este chip opera de manera similar al chip "RAM64", con la diferencia de que en lugar de usar chips "RAM8", utiliza chips "RAM64" en su lugar. Además, la entrada "address" ahora tiene 9 dígitos: los primeros 3 indican en qué "RAM64" se almacenará el dato y los otros 6 bits se ingresan a la RAM64 correspondiente.

# RAM4K
Este chip funciona de la misma manera que el chip "RAM512", pero en lugar de usar chips "RAM64", utiliza chips "RAM512". El "address" ahora tiene 12 dígitos: los tres primeros indican a cuál "RAM512" irá el dato y los otros 9 van a la RAM512 específica.

# RAM16K
El chip "RAM16K" sigue una estructura similar al chip anterior, pero en este caso, en lugar de utilizar 8 veces el chip anterior, utiliza solo 4 veces. Para acceder a la memoria, se utiliza una dirección de 14 dígitos. Los dos primeros dígitos seleccionan la RAM4K en la que se almacenará el dato, y los otros 12 dígitos se dirigen al chip RAM4K específico.
