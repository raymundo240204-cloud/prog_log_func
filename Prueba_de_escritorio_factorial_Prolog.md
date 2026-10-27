# Prueba de escritorio: factorial en Prolog

**Materia:** Programación Lógica y Funcional  
**Tema:** Recursividad y cálculo del factorial en Prolog

## Objetivo

En esta actividad hice la prueba de escritorio de un programa en Prolog que calcula el factorial de un número. Usé el número 5 para revisar paso a paso cómo se realizan las llamadas recursivas y cómo se obtiene el resultado.

## Código del programa

```prolog
% Caso base
factorial(0, 1) :- !.

% Caso recursivo
factorial(N, F) :-
    N > 0,
    N1 is N - 1,
    factorial(N1, F1),
    F is N * F1.
```

El caso base establece que el factorial de 0 es 1. El símbolo `!` se llama *corte* y evita que Prolog intente otras alternativas una vez que encuentra ese caso. En el caso recursivo se resta 1 a `N` y se vuelve a llamar al predicado hasta llegar a 0. Después se hacen las multiplicaciones al regresar de cada llamada.

## Consulta utilizada

```prolog
?- factorial(5, F).
```

## Prueba de escritorio: llamadas recursivas

Primero anoté cómo va disminuyendo el valor de `N` hasta alcanzar el caso base.

| Paso | N | N1 = N - 1 | ¿Qué hace el programa? |
|---|---:|---:|---|
| 1 | 5 | 4 | Llama a `factorial(4, F1)`. |
| 2 | 4 | 3 | Llama a `factorial(3, F1)`. |
| 3 | 3 | 2 | Llama a `factorial(2, F1)`. |
| 4 | 2 | 1 | Llama a `factorial(1, F1)`. |
| 5 | 1 | 0 | Llama a `factorial(0, F1)`. |
| 6 | 0 | — | Llega al caso base: el resultado es 1. |

En esta primera parte todavía no se calcula el resultado final, porque las multiplicaciones quedan pendientes hasta terminar las llamadas recursivas.

## Prueba de escritorio: regreso de las llamadas

Después del caso base, el programa regresa y realiza las multiplicaciones que quedaron pendientes. `F1` representa el factorial del número anterior.

| Paso | N | F1 (resultado anterior) | Operación | F |
|---|---:|---:|---|---:|
| 7 | 1 | 1 | 1 × 1 | 1 |
| 8 | 2 | 1 | 2 × 1 | 2 |
| 9 | 3 | 2 | 3 × 2 | 6 |
| 10 | 4 | 6 | 4 × 6 | 24 |
| 11 | 5 | 24 | 5 × 24 | 120 |

**Secuencia completa:**

```text
factorial(5)
  factorial(4)
    factorial(3)
      factorial(2)
        factorial(1)
          factorial(0) = 1
        factorial(1) = 1 * 1  = 1
      factorial(2) = 2 * 1  = 2
    factorial(3) = 3 * 2    = 6
  factorial(4) = 4 * 6      = 24
factorial(5) = 5 * 24       = 120
```

## Resultado

Al ejecutar la consulta, el resultado esperado es:

```prolog
?- factorial(5, F).
F = 120.
```

Este resultado coincide con la operación matemática `5! = 5 × 4 × 3 × 2 × 1 = 120`.

## Conclusión

Con esta prueba de escritorio entendí mejor cómo funciona la recursividad en Prolog. Al principio pensé que las multiplicaciones se hacían mientras el número iba bajando, pero al seguir el código vi que primero se llega al caso base y luego se calculan los resultados de regreso. Para el número 5, el programa terminó dando 120, así que la prueba de escritorio coincide con el factorial que se obtiene matemáticamente.
