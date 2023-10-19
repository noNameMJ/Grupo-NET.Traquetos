# FILL

Este programa verifica continuamente la entrada de teclado y llena la pantalla de negro cuando se presiona una tecla y la borra cuando no se presiona ninguna tecla.

#### Inicialización (BEGIN)
1. `@24576`: El programa establece la dirección de memoria en 24576 para la entrada de teclado. (lo mismo se puede hacer con `@screen`)
2. `D=A`: Almacena el valor de 24576 (la dirección para la entrada de teclado) en el registro de datos (D).
3. `@keyboard`: Establece la dirección para la entrada de teclado (una variable) en el valor del registro de datos (D).

#### Bucle de Comprobación de Entrada de Teclado (CHECK_KEYBOARD)
1. `@24575`: El programa establece la dirección de memoria en 24575 para el último mapa de píxeles de pantalla.
2. `D=A`: Almacena el valor de 24575 (la dirección para el último mapa de píxeles) en el registro de datos (D).
3. `@current`: Establece la dirección para el mapa de píxeles actual (una variable) en el valor del registro de datos (D).

#### Comprobar la Entrada de Teclado
1. `@keyboard`: Obtiene el valor de la entrada de teclado.
2. `A=M`: Va a la dirección de memoria especificada por la entrada de teclado.
3. `D=M`: Almacena el valor en esa dirección de memoria (valor de la entrada de teclado) en el registro de datos (D).
4. `@fillvalue`: El programa establece la dirección para una variable llamada `fillvalue`.
5. `M=-1`: Si se presiona una tecla (el valor de la entrada de teclado no es cero), establece `fillvalue` en -1.
6. `@DRAW`: Salta a la sección DRAW para llenar o borrar la pantalla según `fillvalue`.
7. `D;JNE`: Si D (valor de la entrada de teclado) no es cero (se presiona una tecla), salta a la sección DRAW.

#### Borrar la Pantalla (DRAW)
1. `@fillvalue`: El programa obtiene el valor de `fillvalue`.
2. `D=M`: Almacena el valor de `fillvalue` en el registro de datos (D).
3. `@current`: Obtiene el valor de la dirección del mapa de píxeles actual.
4. `A=M`: Va a la dirección de memoria especificada por el mapa de píxeles actual.
5. `M=D`: Establece el valor del píxel (negro o blanco) en función de `fillvalue`.

#### Continuar el Bucle
1. `@current`: El programa obtiene el valor del mapa de píxeles actual.
2. `D=M`: Almacena el valor del mapa de píxeles actual en el registro de datos (D).
3. `@16384`: El programa establece la dirección para el primer mapa de píxeles.
4. `D=D-A`: Calcula la diferencia entre el mapa de píxeles actual y el primer mapa de píxeles.
5. `@CHECK_KEYBOARD`: Salta de nuevo a la sección CHECK_KEYBOARD para continuar monitoreando la entrada de teclado.
6. `D;JLE`: Si el mapa de píxeles actual es menor o igual al primer mapa de píxeles, continúa con el siguiente mapa de píxeles.
7. `@current`: Decrementa el valor del mapa de píxeles actual.
8. `M=M-1`: Actualiza el mapa de píxeles actual.
9. `@DRAW`: Salta de nuevo a la sección DRAW para continuar llenando o borrando píxeles.

# Mult 

El código de mult funciona de la siguiente manera:
 1. Se tiene una variable, en nuestro caso en la posición de memoria 2, que contiene el resultado de la multiplicación
 2. Se tiene una variable, en nuestro caso en la posición de memoria 1, que contiene el multiplicando
 3. Se tiene una variable, en nuestro caso en la posición de memoria 0, que contiene el multiplicador

Creamos un iterador i, que va a llevar la cuenta de las veces que se ha sumado el multiplicando, y lo inicializamos a 1.

Lo primero que hacemos es verificar cual de los 2, es el mayor, y utlizando esto, hacemos el número de iteraciones igual al menor, y sumamos el mayor, el número de veces del menor.

Para hacer esto, hacemos una resta de los valores. Así
```hack
@0
D=M
@1
D=D-M
```

Por lo que si el valor es negativo, el mayor es el valor en la posición 1, y si es positivo, el mayor es el valor en la posición 0. Luego, pasamos a la parte de la suma.

Tomando la variable que se encuentra en el mayor, iteramos i veces, y sumamos el valor del menor a la variable que contiene el resultado. Para verificar si el iterador terminó, se le resta a i el valor del menor, y si es positivo, significa que el iterador superó al menor, así que termina la iteración.
