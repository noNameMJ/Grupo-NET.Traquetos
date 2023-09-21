# HalfAdder

Para el HalfAdder, primero podemos observar que necesitamos obtener la siguiente tabla:

| a | b | out | carry |
|---|---|-----|-------|
| 0 | 0 |  0  |   0   |
| 0 | 1 |  1  |   0   |
| 1 | 0 |  1  |   0   |
| 1 | 1 |  0  |   1   |

Como se puede notar, el resultado "out" corresponde a una compuerta XOR, mientras que "carry" es el resultado de una compuerta AND. Así, las salidas deseadas son out = XOR(a,b) y carry = AND(a,b).

# FullAdder

Como ejemplo, se puede ver que la suma máxima es 10, pero para representar 2 bits, necesitamos alcanzar 11, que es igual a 3. Por lo tanto, necesitamos una función que pueda sumar hasta 3. Para lograrlo, podemos usar un HalfAdder para sumar nuestras entradas "a" y "b", lo que nos da un resultado llamado "out" y un valor de acarreo. Luego, sumamos un tercer elemento, "c", para permitir sumar hasta 3. El acarreo de la suma entre "a" y "b" se combina con la suma de "out" y "c" usando una compuerta OR, ya que tenemos 3 posibilidades:

1. Carry1 = 1, carry2 = 0, total 1.
2. Carry1 = 0, Carry2 = 1, total 1.
3. Carry1 = 0, Carry2 = 0, total 0.

Por lo tanto, si al menos uno de los dos acarreos es 1, el acarreo total es 1. El caso en el que ambos acarreos sean 1 no es posible, ya que eso significaría que debemos sumar hasta 4, lo cual no es el caso aquí.

# Add16

En este escenario, estamos sumando dos entradas de 16 bits cada una. En el primer paso, aplicamos un HalfAdder, ya que sabemos que el valor máximo que puede dar es 10. Luego, el acarreo de esta suma se lleva a las siguientes etapas, donde usamos FullAdders para sumar los bits correspondientes de "a" y "b" junto con el acarreo de la operación anterior, lo que da como resultado la suma de las dos entradas de 16 bits.

# Inc16

Para realizar un incremento de 16 bits (Inc16), debemos sumar "0000000000000001" a una entrada de 16 bits. Sabemos que la representación binaria de "verdadero" se compone de 16 unos, es decir, "1111111111111111", que es igual a -1 en decimal. Si sumamos esto a otro "verdadero", obtenemos "-2". Sin embargo, si negamos esta entrada, obtenemos "0000000000000001", que es el valor que necesitamos sumar. Luego, simplemente sumamos esta entrada con nuestro "uno" y obtenemos el resultado deseado.

# ALU

En la unidad aritmético-lógica (ALU), debemos realizar múltiples operaciones:

Si alguno (o ambos) de los bits "zx" o "zy" son 1, debemos inicializar la correspondiente entrada como 16 ceros.
Si alguno (o ambos) de los bits "nx" o "ny" son 1, debemos negar la entrada correspondiente.
Si el bit "f" es 1, debemos realizar una suma en las entradas. De lo contrario, debemos aplicar una operación AND en las entradas.
Si el bit "no" no es 1, la salida debe negarse. De lo contrario, la salida se mantiene sin cambios.
Dado que estas instrucciones se aplican en secuencia, primero se aplican "zx" y "zy".

Podemos notar que un multiplexor funciona como una estructura condicional en programación, donde si es verdadero, realiza una acción, y si es falso, realiza otra. Para implementarlo, primero calculamos todas las posibles salidas. En el caso de "zx", calculamos "x" y una fila de ceros. Luego, pasamos estos valores calculados al multiplexor y elegimos uno de ellos en función del selector "zx". Realizamos un proceso similar para "zy", "nx", "ny", "f" y "no", organizándolos en forma de árbol.

<img src="/Images/alu.png">

Finalmente, consideramos dos salidas adicionales, "zr" y "ng". Para "zr", simplemente verificamos si alguno de los bits en la salida es igual a 1 usando una operación OR en cadena (usando "or8way"). Luego, negamos el resultado. Esto significa que si al menos uno de los bits es 1, la salida es falsa; de lo contrario, es verdadera.

Para "ng", tomamos el bit más significativo de la salida, "out[15]", que representa el signo, y lo comparamos con una operación AND utilizando un valor "true". Esto hace que la salida dependa solo del signo. Si es igual a 1, el resultado es negativo; de lo contrario, es positivo.
