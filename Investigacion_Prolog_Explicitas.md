# Investigación: funciones y predicados de Prolog

**Materia:** Programación Lógica y Funcional  
**Tema:** Funciones y predicados principales de Prolog  
**Lenguaje utilizado como referencia:** SWI-Prolog

## Introducción

En esta investigación revisé algunas de las funciones y predicados que se utilizan en Prolog. Este lenguaje es diferente a otros como Java o Python porque, en lugar de indicar todos los pasos que debe seguir el programa, podemos escribir hechos y reglas para después hacer consultas.

Algo que me pareció importante es que en Prolog muchas de las operaciones que normalmente llamaríamos *funciones* en realidad se conocen como **predicados**. Un predicado sirve para comprobar o relacionar datos. También existen funciones aritméticas, como `abs` y `sqrt`, que se usan para realizar cálculos.

Cuando aparece un nombre como `member/2`, el número después de la diagonal indica cuántos argumentos recibe el predicado.

## 1. Predicados para mostrar y leer información

Estos sirven para comunicarnos con el usuario desde la consola.

| Predicado | ¿Para qué sirve? | Ejemplo |
|---|---|---|
| `write/1` | Muestra un dato o mensaje. | `write('Hola').` |
| `writeln/1` | Muestra un dato y pasa a la siguiente línea. | `writeln('Hola').` |
| `nl/0` | Agrega un salto de línea. | `nl.` |
| `read/1` | Permite leer un término que escribe el usuario. | `read(X).` |
| `format/2` | Permite mostrar información con un formato. | `format('Numero: ~w~n', [5]).` |

**Ejemplo:** si escribimos `write('Bienvenido').`, Prolog muestra el mensaje `Bienvenido`.

## 2. Predicados para trabajar con listas

En Prolog las listas se escriben entre corchetes, por ejemplo `[1,2,3]`. Los siguientes predicados ayudan a buscar elementos y modificar el orden de los datos.

| Predicado | ¿Qué hace? | Ejemplo de consulta | Resultado |
|---|---|---|---|
| `member/2` | Revisa si un elemento está en una lista. | `member(2, [1,2,3]).` | `true` |
| `length/2` | Obtiene la cantidad de elementos. | `length([a,b,c], N).` | `N = 3` |
| `append/3` | Une dos listas. | `append([1,2], [3,4], R).` | `R = [1,2,3,4]` |
| `reverse/2` | Invierte una lista. | `reverse([1,2,3], R).` | `R = [3,2,1]` |
| `nth0/3` | Busca el elemento de una posición, contando desde cero. | `nth0(1, [a,b,c], X).` | `X = b` |
| `sort/2` | Ordena una lista y elimina los repetidos. | `sort([3,1,3,2], R).` | `R = [1,2,3]` |

Por ejemplo, `reverse/2` me parece útil cuando necesitamos recorrer los datos en orden contrario, sin tener que crear todo el procedimiento desde cero.

## 3. Operadores de comparación y operaciones matemáticas

También encontré operadores que permiten comparar valores o realizar cálculos. Una diferencia importante es que **`=` no hace lo mismo que `is`**: el primero une términos y el segundo evalúa una operación matemática.

| Operador | Uso | Ejemplo |
|---|---|---|
| `=` | Unifica dos términos. | `X = 5.` |
| `\=` | Comprueba que dos términos no se pueden unificar. | `a \= b.` |
| `==` | Comprueba si dos términos son idénticos. | `2 == 2.` |
| `is` | Calcula una expresión. | `X is 3 + 4.` → `X = 7` |
| `=:=` | Compara si dos expresiones tienen el mismo valor. | `2 + 3 =:= 5.` |
| `=\=` | Revisa si dos valores numéricos son diferentes. | `2 =\= 3.` |
| `>` y `<` | Compara si un número es mayor o menor. | `5 > 3.` |
| `>=` y `=<` | Compara mayor o igual, o menor o igual. | `4 =< 6.` |

Entre las funciones aritméticas más utilizadas están las siguientes:

| Función u operador | Descripción | Ejemplo |
|---|---|---|
| `abs/1` | Obtiene el valor absoluto. | `X is abs(-8).` → `X = 8` |
| `sqrt/1` | Calcula una raíz cuadrada. | `X is sqrt(25).` → `X = 5.0` |
| `max/2` | Devuelve el número mayor. | `X is max(4,9).` → `X = 9` |
| `min/2` | Devuelve el número menor. | `X is min(4,9).` → `X = 4` |
| `mod` | Calcula el residuo de una división. | `X is 10 mod 3.` → `X = 1` |
| `//` | Realiza división entera. | `X is 10 // 3.` → `X = 3` |
| `**` | Calcula potencias. | `X is 2 ** 3.` → `X = 8.0` |

**Ejemplo para entender `is`:**

```prolog
?- Resultado is 10 + 5.
Resultado = 15.
```

Aquí sí se hace la suma y se guarda el resultado en la variable.

## 4. Predicados lógicos y de control

Estos predicados ayudan a decidir si una consulta se cumple y a controlar las posibles respuestas.

| Predicado u operador | ¿Para qué sirve? |
|---|---|
| `true/0` | Siempre tiene éxito. |
| `fail/0` | Siempre falla. |
| `\+/1` | Comprueba que no se puede demostrar una condición. |
| `,` | Funciona como un **Y** lógico. |
| `;` | Funciona como un **O** lógico. |
| `!/0` | Es el operador de corte, que limita la búsqueda de otras soluciones. |
| `findall/3` | Guarda todas las soluciones encontradas en una lista. |
| `bagof/3` | Agrupa las soluciones, tomando en cuenta las variables libres. |
| `setof/3` | Reúne las soluciones ordenadas y sin repetir elementos. |

Un ejemplo de `findall/3` sería:

```prolog
?- findall(X, member(X, [1,2,3]), R).
R = [1,2,3].
```

En este caso Prolog reúne en `R` los valores que va encontrando.

## 5. Predicados para identificar tipos de datos

Estos sirven para comprobar qué tipo de información tenemos.

| Predicado | Función | Ejemplo |
|---|---|---|
| `var/1` | Revisa si es una variable sin valor asignado. | `var(X).` |
| `nonvar/1` | Revisa si ya no es una variable libre. | `nonvar(hola).` |
| `integer/1` | Comprueba si es un entero. | `integer(10).` |
| `number/1` | Comprueba si es un número. | `number(3.5).` |
| `atom/1` | Comprueba si es un átomo. | `atom(juan).` |
| `is_list/1` | Comprueba si es una lista. | `is_list([a,b]).` |

## 6. Ejemplos de reglas creadas por nosotros

Además de utilizar los predicados que trae Prolog, también podemos escribir nuestros propios predicados utilizando hechos y reglas.

### Ejemplo 1. Relaciones familiares

```prolog
% Hechos
padre(juan, ana).
padre(juan, luis).
madre(maria, ana).
madre(maria, luis).

% Reglas
progenitor(X, Y) :- padre(X, Y).
progenitor(X, Y) :- madre(X, Y).

hermanos(X, Y) :-
    progenitor(P, X),
    progenitor(P, Y),
    X \= Y.
```

Consultas que podemos hacer:

```prolog
?- padre(juan, ana).
true.

?- hermanos(ana, luis).
true.
```

En este ejemplo primero escribimos los datos que ya sabemos y después creamos una regla para que el programa identifique si dos personas tienen un progenitor en común. La regla puede encontrar la misma pareja más de una vez si comparten dos progenitores.

### Ejemplo 2. Suma de dos números

```prolog
suma(A, B, Resultado) :-
    Resultado is A + B.
```

Consulta:

```prolog
?- suma(6, 4, R).
R = 10.
```

Con esto podemos crear un predicado que recibe dos números y devuelve su suma.

### Ejemplo 3. Factorial usando recursividad

```prolog
factorial(0, 1).
factorial(N, Resultado) :-
    integer(N),
    N > 0,
    N1 is N - 1,
    factorial(N1, Parcial),
    Resultado is N * Parcial.
```

Consulta:

```prolog
?- factorial(5, R).
R = 120.
```

Este ejemplo me ayudó a entender cómo funciona la recursividad en Prolog: se vuelve a utilizar la misma regla con un número menor hasta llegar al caso base, que es el factorial de cero.

## 7. ¿Cómo se ejecutan los ejemplos?

Para probar este tipo de código se puede utilizar **SWI-Prolog**. Primero se guardan los hechos y reglas en un archivo con extensión `.pl`, por ejemplo `ejemplos.pl`. Después, en la consola de Prolog, se carga con:

```prolog
?- [ejemplos].
```

Una vez cargado, se pueden escribir las consultas, como `?- suma(6, 4, R).`, siempre que hayamos incluido antes esa regla en el archivo. El símbolo `?-` indica que estamos escribiendo una consulta en la consola y no debe incluirse dentro del archivo `.pl`.

## Conclusión

Con esta investigación comprendí que Prolog trabaja principalmente con predicados, hechos, reglas y consultas. También aprendí que no todas sus operaciones funcionan como las funciones de otros lenguajes: algunas sirven para relacionar valores, otras para comparar datos y otras para realizar cálculos.

Los predicados que considero más útiles para comenzar son `write`, `member`, `length`, `append` e `is`, porque permiten mostrar información, trabajar con listas y hacer operaciones matemáticas. Los ejemplos también me ayudaron a identificar cómo se pueden crear reglas propias y utilizar la recursividad para resolver problemas.

## Fuentes consultadas

- SWI-Prolog. *Built-in Predicates*. https://www.swi-prolog.org/pldoc/man?section=builtin
- SWI-Prolog. *Quick Start*. https://www.swi-prolog.org/man/quickstart.html
- SWI-Prolog. *Documentación oficial*. https://www.swi-prolog.org/pldoc/

**Nota:** Los ejemplos toman como referencia SWI-Prolog. Algunos predicados pueden cambiar dependiendo de la versión o implementación utilizada.
