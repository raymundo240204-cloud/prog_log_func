# 25 funciones primitivas de Common Lisp en CLISP

> Sintaxis, funcionamiento, resultados y ejemplos prácticos.

**Nombre:** ____________________________________  
**Asignatura:** _________________________________  
**Fecha:** _____________________________________

## Introducción

GNU CLISP es una implementación de Common Lisp que sigue ampliamente el estándar ANSI. El [Common Lisp HyperSpec](https://www.lispworks.com/documentation/HyperSpec/Front/index.htm) es la referencia técnica principal utilizada en esta investigación.

Aunque en cursos introductorios se habla de **funciones primitivas**, el estándar no define una categoría formal con ese nombre. En este documento la expresión se utiliza para referirse a operaciones básicas incorporadas en el lenguaje. Se seleccionaron funciones y accesores invocables como funciones; se dejaron fuera macros, tipos y operadores especiales.

## Cómo leer las fichas

- **Sintaxis:** muestra la forma general de invocación. `&rest` indica que se aceptan cero o más argumentos adicionales.
- **Resultado:** identifica qué devuelve la función. En Common Lisp, `NIL` representa tanto falso como la lista vacía.
- **Ejemplo:** utiliza `=>` para indicar el valor producido al evaluar una expresión.
- **Observación:** presenta un caso especial, una precaución o una diferencia importante.

## Contenido

1. Manejo de listas — 5 funciones
2. Secuencias y listas — 4 funciones
3. Predicados de tipo — 4 funciones
4. Comparación — 4 funciones
5. Aritmética — 6 funciones
6. Aplicación de funciones — 2 funciones

## Manejo de listas

### 1. `CONS`

**Sintaxis**

```lisp
(cons objeto-1 objeto-2)
```

**Funcionamiento:** Crea una nueva celda cons cuyo CAR es el primer objeto y cuyo CDR es el segundo. Se utiliza para construir pares, listas propias y listas punteadas.

**Resultado:** Devuelve una nueva celda cons.

**Ejemplo**

```lisp
(cons 'a '(b c))  =>  (A B C)
```

> **Observación:** Si el segundo argumento no es una lista, el resultado es un par punteado: (cons 'a 'b) => (A . B).

### 2. `CAR`

**Sintaxis**

```lisp
(car lista)
```

**Funcionamiento:** Obtiene el primer componente de una celda cons. En una lista común corresponde al primer elemento.

**Resultado:** Devuelve el objeto almacenado en la parte CAR; para NIL devuelve NIL.

**Ejemplo**

```lisp
(car '(rojo verde azul))  =>  ROJO
```

> **Observación:** Aplicar CAR a un objeto que no sea una lista es un error de tipo.

### 3. `CDR`

**Sintaxis**

```lisp
(cdr lista)
```

**Funcionamiento:** Obtiene el segundo componente de una celda cons. En una lista común corresponde a la lista formada por los elementos restantes.

**Resultado:** Devuelve el objeto almacenado en la parte CDR; para NIL devuelve NIL.

**Ejemplo**

```lisp
(cdr '(rojo verde azul))  =>  (VERDE AZUL)
```

> **Observación:** CDR no copia la lista: devuelve la cola que ya forma parte de la estructura original.

### 4. `LIST`

**Sintaxis**

```lisp
(list &rest objetos)
```

**Funcionamiento:** Construye una lista propia que contiene los argumentos en el mismo orden en que fueron proporcionados.

**Resultado:** Devuelve una lista nueva; sin argumentos devuelve NIL.

**Ejemplo**

```lisp
(list 'a 2 "tres")  =>  (A 2 "tres")
```

> **Observación:** Es una forma cómoda de construir datos sin escribir explícitamente varias llamadas a CONS.

### 5. `APPEND`

**Sintaxis**

```lisp
(append &rest listas)
```

**Funcionamiento:** Concatena listas. Copia la estructura de todas las listas excepto la última y conserva intactos los argumentos originales.

**Resultado:** Devuelve el objeto concatenado; normalmente es una lista.

**Ejemplo**

```lisp
(append '(a b) '(c d))  =>  (A B C D)
```

> **Observación:** La última lista no se copia. También puede producir una lista punteada si el último argumento no es una lista.

## Secuencias y listas

### 6. `LENGTH`

**Sintaxis**

```lisp
(length secuencia)
```

**Funcionamiento:** Cuenta los elementos de una secuencia, como una lista, una cadena o un vector.

**Resultado:** Devuelve un entero no negativo.

**Ejemplo**

```lisp
(length '(10 20 30 40))  =>  4
```

> **Observación:** Para listas, el argumento debe ser una lista propia; una lista circular no tiene longitud finita.

### 7. `REVERSE`

**Sintaxis**

```lisp
(reverse secuencia)
```

**Funcionamiento:** Crea una secuencia con los mismos elementos en orden inverso. La secuencia de entrada no se modifica.

**Resultado:** Devuelve una secuencia nueva del mismo tipo general que la original.

**Ejemplo**

```lisp
(reverse '(1 2 3 4))  =>  (4 3 2 1)
```

> **Observación:** NREVERSE es la variante potencialmente destructiva; para conservar el original debe preferirse REVERSE.

### 8. `NTH`

**Sintaxis**

```lisp
(nth n lista)
```

**Funcionamiento:** Obtiene el elemento que ocupa el índice n de una lista. La numeración comienza en cero.

**Resultado:** Devuelve el elemento encontrado o NIL cuando el índice rebasa una lista propia.

**Ejemplo**

```lisp
(nth 2 '(a b c d))  =>  C
```

> **Observación:** NTH equivale conceptualmente a aplicar CAR al resultado de NTHCDR.

### 9. `MEMBER`

**Sintaxis**

```lisp
(member elemento lista &key key test test-not)
```

**Funcionamiento:** Busca un elemento en una lista. Por defecto compara con EQL y permite personalizar la comparación mediante palabras clave.

**Resultado:** Devuelve la cola de la lista que comienza con la coincidencia, o NIL si no encuentra ninguna.

**Ejemplo**

```lisp
(member 'c '(a b c d))  =>  (C D)
```

> **Observación:** No devuelve simplemente T: la cola devuelta permite saber dónde se produjo la coincidencia.

## Predicados de tipo

### 10. `ATOM`

**Sintaxis**

```lisp
(atom objeto)
```

**Funcionamiento:** Comprueba si un objeto no es una celda cons. Todo número, símbolo, cadena y también NIL es un átomo.

**Resultado:** Devuelve T si el objeto es un átomo; de lo contrario devuelve NIL.

**Ejemplo**

```lisp
(atom 'hola)  =>  T     |     (atom '(a b))  =>  NIL
```

> **Observación:** NIL es simultáneamente un átomo y una lista vacía.

### 11. `NULL`

**Sintaxis**

```lisp
(null objeto)
```

**Funcionamiento:** Comprueba específicamente si el objeto es NIL. Es útil para expresar con claridad que se espera el final o la ausencia de una lista.

**Resultado:** Devuelve T únicamente para NIL; en cualquier otro caso devuelve NIL.

**Ejemplo**

```lisp
(null '())  =>  T     |     (null '(a))  =>  NIL
```

> **Observación:** Tiene el mismo comportamiento lógico que NOT, pero comunica mejor la intención al trabajar con listas.

### 12. `LISTP`

**Sintaxis**

```lisp
(listp objeto)
```

**Funcionamiento:** Determina si un objeto pertenece al tipo LIST, es decir, si es una celda cons o NIL.

**Resultado:** Devuelve T para listas y NIL para los demás objetos.

**Ejemplo**

```lisp
(listp '(1 2))  =>  T     |     (listp 25)  =>  NIL
```

> **Observación:** Un cons punteado también satisface LISTP; la función no garantiza que la lista sea propia.

### 13. `NUMBERP`

**Sintaxis**

```lisp
(numberp objeto)
```

**Funcionamiento:** Comprueba si un objeto es un número de Common Lisp, incluidos enteros, razones, flotantes y complejos.

**Resultado:** Devuelve T cuando el objeto es numérico y NIL en caso contrario.

**Ejemplo**

```lisp
(numberp 3.14)  =>  T     |     (numberp "3.14")  =>  NIL
```

> **Observación:** Una cadena que contiene dígitos no es un número hasta que se convierte o analiza.

## Comparación

### 14. `EQ`

**Sintaxis**

```lisp
(eq x y)
```

**Funcionamiento:** Comprueba identidad de objeto. Es especialmente apropiada para símbolos y para saber si dos referencias apuntan exactamente al mismo objeto.

**Resultado:** Devuelve T si x e y son el mismo objeto; de lo contrario devuelve NIL.

**Ejemplo**

```lisp
(eq 'lisp 'lisp)  =>  T
```

> **Observación:** No debe usarse para comparar de manera portátil números o caracteres; para esos casos se recomienda EQL.

### 15. `EQL`

**Sintaxis**

```lisp
(eql x y)
```

**Funcionamiento:** Extiende EQ para comparar de forma fiable números del mismo tipo y con el mismo valor, así como caracteres equivalentes.

**Resultado:** Devuelve T cuando los objetos son EQL; en otro caso devuelve NIL.

**Ejemplo**

```lisp
(eql 10 10)  =>  T     |     (eql 10 10.0)  =>  NIL
```

> **Observación:** Dos números de tipos distintos no son EQL aunque representen el mismo valor matemático.

### 16. `EQUAL`

**Sintaxis**

```lisp
(equal x y)
```

**Funcionamiento:** Realiza una comparación estructural. Examina recursivamente listas y compara el contenido de cadenas y otros objetos definidos por el estándar.

**Resultado:** Devuelve T cuando las estructuras son equivalentes según EQUAL.

**Ejemplo**

```lisp
(equal '(a (b c)) '(a (b c)))  =>  T
```

> **Observación:** La comparación de caracteres en cadenas distingue mayúsculas y minúsculas.

### 17. `EQUALP`

**Sintaxis**

```lisp
(equalp x y)
```

**Funcionamiento:** Es la comparación general más permisiva: compara estructuras recursivamente, caracteres sin distinguir mayúsculas y números sin exigir el mismo tipo.

**Resultado:** Devuelve T cuando los objetos son equivalentes según las reglas de EQUALP.

**Ejemplo**

```lisp
(equalp "Lisp" "LISP")  =>  T
```

> **Observación:** También compara elemento por elemento ciertos arreglos y estructuras, por lo que puede realizar más trabajo que EQ o EQL.

## Aritmética

### 18. `+`

**Sintaxis**

```lisp
(+ &rest números)
```

**Funcionamiento:** Suma todos los argumentos de izquierda a derecha.

**Resultado:** Devuelve la suma; sin argumentos devuelve 0.

**Ejemplo**

```lisp
(+ 2 3 5)  =>  10
```

> **Observación:** Admite los distintos tipos numéricos de Common Lisp y aplica las reglas de combinación numérica del lenguaje.

### 19. `-`

**Sintaxis**

```lisp
(- número &rest más-números)
```

**Funcionamiento:** Con un argumento calcula su opuesto; con varios resta cada argumento posterior al primero.

**Resultado:** Devuelve la negación o la diferencia correspondiente.

**Ejemplo**

```lisp
(- 10 3 2)  =>  5     |     (- 7)  =>  -7
```

> **Observación:** A diferencia de +, requiere al menos un argumento.

### 20. `*`

**Sintaxis**

```lisp
(* &rest números)
```

**Funcionamiento:** Multiplica todos los argumentos.

**Resultado:** Devuelve el producto; sin argumentos devuelve 1.

**Ejemplo**

```lisp
(* 2 3 4)  =>  24
```

> **Observación:** El valor 1 sin argumentos corresponde al elemento identidad de la multiplicación.

### 21. `/`

**Sintaxis**

```lisp
(/ número &rest divisores)
```

**Funcionamiento:** Con un argumento obtiene su recíproco; con varios divide el primero sucesivamente entre los demás.

**Resultado:** Devuelve el cociente correspondiente.

**Ejemplo**

```lisp
(/ 20 2 5)  =>  2     |     (/ 4)  =>  1/4
```

> **Observación:** Dividir entre cero provoca una condición de división por cero.

### 22. `ABS`

**Sintaxis**

```lisp
(abs número)
```

**Funcionamiento:** Calcula el valor absoluto de un número real o la magnitud de un número complejo.

**Resultado:** Devuelve un número real no negativo.

**Ejemplo**

```lisp
(abs -12)  =>  12
```

> **Observación:** Para un complejo, el resultado representa su magnitud y puede ser un número en punto flotante.

### 23. `MOD`

**Sintaxis**

```lisp
(mod número divisor)
```

**Funcionamiento:** Calcula el residuo de la división. El resultado se define a partir de FLOOR y conserva el signo del divisor cuando no es cero.

**Resultado:** Devuelve el residuo de la división.

**Ejemplo**

```lisp
(mod -13 4)  =>  3
```

> **Observación:** Puede diferir de REM con números negativos, pues REM se relaciona con TRUNCATE.

## Aplicación de funciones

### 24. `APPLY`

**Sintaxis**

```lisp
(apply función &rest argumentos)
```

**Funcionamiento:** Invoca una función expandiendo como argumentos individuales los elementos de la última lista recibida. Puede haber argumentos explícitos antes de esa lista.

**Resultado:** Devuelve los mismos valores que produzca la función llamada.

**Ejemplo**

```lisp
(apply #'+ '(1 2 3 4))  =>  10
```

> **Observación:** El último argumento de APPLY debe ser una lista que pueda distribuirse como argumentos.

### 25. `FUNCALL`

**Sintaxis**

```lisp
(funcall función &rest argumentos)
```

**Funcionamiento:** Invoca una función cuyos argumentos ya se proporcionan individualmente. La función puede indicarse mediante un símbolo o un objeto función.

**Resultado:** Devuelve los mismos valores que produzca la función llamada.

**Ejemplo**

```lisp
(funcall #'* 6 7)  =>  42
```

> **Observación:** Use FUNCALL cuando los argumentos ya están separados; use APPLY cuando la parte final se encuentra dentro de una lista.

## Conclusiones

Las funciones estudiadas muestran cinco ideas centrales de Common Lisp: las listas se construyen a partir de celdas *cons*; las secuencias tienen operaciones reutilizables; los predicados devuelven valores lógicos; existen distintos niveles de igualdad; y las funciones son objetos que pueden enviarse a `APPLY` y `FUNCALL`.

Comprender estas diferencias evita errores comunes, como usar `EQ` para comparar números, esperar que `MEMBER` devuelva únicamente `T` o asumir que `APPEND` copia su último argumento.

## Fuentes consultadas

1. [Common Lisp HyperSpec: índice alfabético de símbolos](https://www.lispworks.com/documentation/HyperSpec/Front/X_Symbol.htm)
2. [Common Lisp HyperSpec: diccionario de conses y listas](https://www.lispworks.com/documentation/HyperSpec/Body/c_conses.htm)
3. [Common Lisp HyperSpec: diccionario de secuencias](https://www.lispworks.com/documentation/HyperSpec/Body/c_sequen.htm)
4. [Common Lisp HyperSpec: diccionario de números](https://www.lispworks.com/documentation/HyperSpec/Body/c_number.htm)
5. [Common Lisp HyperSpec: datos y flujo de control](https://www.lispworks.com/documentation/HyperSpec/Body/c_data_a.htm)
6. [Common Lisp HyperSpec: función `APPEND`](https://www.lispworks.com/documentation/HyperSpec/Body/f_append.htm#append)
7. [GNU CLISP: notas oficiales de implementación](https://www.gnu.org/software/clisp/impnotes.html)
