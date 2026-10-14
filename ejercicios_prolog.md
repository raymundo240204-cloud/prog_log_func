# Ejercicios de programación lógica en Prolog


## 1. Planetas y satélites

```prolog
% La Tierra es un planeta.
planeta(tierra).

% La Luna no es un planeta: negación explícita como predicado separado.
no_es_planeta(luna).

% La Luna es un satélite.
satelite(luna).

% La Tierra gira alrededor del Sol.
gira_alrededor(tierra, sol).

% Todo planeta es un satélite.
satelite(X) :-
    planeta(X).

% Todo planeta gira alrededor del Sol.
gira_alrededor(X, sol) :-
    planeta(X).

% Algún planeta gira alrededor de la Luna.
algun_planeta_gira_alrededor_de_luna :-
    planeta(X),
    gira_alrededor(X, luna).

% Hay por lo menos un satélite.
existe_satelite :-
    satelite(_).
```

`no_es_planeta/1` es un predicado independiente: Prolog no impide automáticamente declarar también `planeta(luna)`. La regla que convierte planetas en satélites reproduce el ejercicio, aunque no sea astronómicamente correcta.

## 2. Perros y carteros

El enunciado «Todos los perros del vecindario muerden a algún…» está incompleto en el documento. Se supone que termina en **cartero**.

```prolog
% Datos de ejemplo.
perro(firulais).
perro(roco).

del_vecindario(firulais).
del_vecindario(roco).

cartero(juan).
cartero(ana).

asusta(firulais, ana).

% Se agrupan los hechos y la regla de muerde/2.
muerde(firulais, juan).
muerde(firulais, ana).
muerde(roco, juan).
muerde(roco, firulais).
muerde(roco, roco).

% Todos los perros que asustan a algún cartero lo muerden.
muerde(P, C) :-
    perro(P),
    cartero(C),
    asusta(P, C).

% Todos los perros del vecindario muerden a algún cartero.
todos_perros_vecindario_muerden_cartero :-
    forall(
        (perro(P), del_vecindario(P)),
        (cartero(C), muerde(P, C))
    ).

% Hay un cartero al que muerden todos los perros.
cartero_mordido_por_todos(C) :-
    cartero(C),
    forall(perro(P), muerde(P, C)).

% Todos los carteros son mordidos por algún perro.
todos_carteros_mordidos :-
    forall(
        cartero(C),
        (perro(P), muerde(P, C))
    ).

% Hay un perro que muerde a todos los carteros.
perro_muerde_todos_carteros(P) :-
    perro(P),
    forall(cartero(C), muerde(P, C)).

% Hay un perro que muerde a todos los perros
% que muerden a algún cartero.
perro_muerde_perros_que_muerden_carteros(P) :-
    perro(P),
    forall(
        (perro(Otro), cartero(C), muerde(Otro, C)),
        muerde(P, Otro)
    ).

% Hay un solo perro que se muerde a sí mismo.
unico_perro_se_muerde(P) :-
    setof(X, (perro(X), muerde(X, X)), [P]).
```

## 3. Biblioteca: datos de ejemplo

Se distingue una obra de sus ejemplares físicos. `prestamo/4` representa préstamos activos e `historial/2` registra préstamos realizados alguna vez. Los días se representan mediante números enteros para simplificar el cálculo del vencimiento.

```prolog
biblioteca(biblioteca_nacional).
biblioteca(biblioteca_central).

libro(don_quijote).
libro(cuentos).
libro(fisica).
libro(manuscrito).

% La Biblioteca Nacional tiene Don Quijote de la Mancha.
tiene(biblioteca_nacional, don_quijote).
tiene(biblioteca_central, don_quijote).
tiene(biblioteca_nacional, cuentos).
tiene(biblioteca_nacional, fisica).
tiene(biblioteca_nacional, manuscrito).

autor(cervantes).
autor(ana_autora).

escribio(cervantes, don_quijote).
escribio(ana_autora, cuentos).
escribio(ana_autora, fisica).
escribio(ana_autora, manuscrito).

publicado(don_quijote).
publicado(cuentos).
publicado(fisica).

buen_estado(don_quijote).
buen_estado(cuentos).
buen_estado(fisica).
mal_estado(manuscrito).

raro(manuscrito).
solo_consulta(manuscrito).

usuario_registrado(luis).
usuario_registrado(maria).
multa(maria).

seccion(literatura).
seccion(infantil).
seccion(ciencias).
seccion(archivo).

en_seccion(don_quijote, literatura).
en_seccion(cuentos, infantil).
en_seccion(fisica, ciencias).
en_seccion(manuscrito, archivo).

de_ciencias(fisica).

paginas(don_quijote, 863).
paginas(cuentos, 80).
paginas(fisica, 250).
paginas(manuscrito, 60).

ejemplar(e1, don_quijote, biblioteca_nacional).
ejemplar(e2, don_quijote, biblioteca_nacional).
ejemplar(e3, cuentos, biblioteca_nacional).
ejemplar(e4, fisica, biblioteca_nacional).
ejemplar(e5, manuscrito, biblioteca_nacional).
ejemplar(e6, don_quijote, biblioteca_central).

% Identificador, usuario, ejemplar y día de salida.
prestamo(p1, luis, e1, 100).

historial(luis, don_quijote).
historial(luis, cuentos).
```

## 4. Biblioteca: reglas y condiciones

Para integrar los enunciados sobre préstamos, las restricciones por multa, libro raro y consulta exclusiva tienen prioridad sobre los permisos generales. Esto es una decisión de modelado para combinar las frases del ejercicio.

```prolog
% Todos los libros de la Biblioteca Nacional están catalogados.
catalogado(L) :-
    libro(L),
    tiene(biblioteca_nacional, L).

% Existen libros que están en más de una biblioteca.
libro_en_varias_bibliotecas(L) :-
    libro(L),
    tiene(B1, L),
    tiene(B2, L),
    dif(B1, B2).

% Si un libro es raro, no se puede prestar.
no_prestable(L) :-
    raro(L).

no_prestable(L) :-
    solo_consulta(L).

% La Biblioteca Central tiene más de 10 000 libros.
% Se cuentan obras distintas, no ejemplares físicos.
central_mas_de_diez_mil :-
    findall(L, tiene(biblioteca_central, L), Lista),
    sort(Lista, Unicos),
    length(Unicos, N),
    N > 10000.

% Todos los autores tienen al menos un libro en una biblioteca.
todos_autores_con_libro_en_biblioteca :-
    forall(
        autor(A),
        (escribio(A, L), libro(L), tiene(_, L))
    ).

% Existe un autor con más de cinco libros publicados.
autor_mas_de_cinco_publicados(A) :-
    autor(A),
    findall(L, (escribio(A, L), publicado(L)), Lista),
    sort(Lista, Unicos),
    length(Unicos, N),
    N > 5.

% No todos los libros de una biblioteca están en buen estado.
no_todos_en_buen_estado(B) :-
    tiene(B, L),
    mal_estado(L).

% Un libro en buen estado puede prestarse,
% salvo las restricciones de raro o solo consulta.
prestable(L) :-
    libro(L),
    buen_estado(L),
    \+ no_prestable(L).

% Un usuario con multa no puede pedir libros.
no_puede_pedir(U) :-
    multa(U).

% Todo usuario con más de tres préstamos
% debe devolver uno para pedir otro.
debe_devolver_uno(U) :-
    usuario_registrado(U),
    findall(Id, prestamo(Id, U, _, _), Lista),
    sort(Lista, Unicos),
    length(Unicos, N),
    N > 3.

% Permiso general para usuarios registrados, sujeto a restricciones.
puede_pedir(U) :-
    usuario_registrado(U),
    \+ no_puede_pedir(U),
    \+ debe_devolver_uno(U).

disponible(E) :-
    ejemplar(E, _, _),
    \+ prestamo(_, _, E, _).

puede_tomar_prestado(U, E) :-
    puede_pedir(U),
    ejemplar(E, L, _),
    prestable(L),
    disponible(E).

% Existen libros que solo se consultan en biblioteca.
existe_libro_solo_consulta :-
    libro(L),
    solo_consulta(L).

% Todo préstamo debe devolverse en 15 días.
fecha_limite(Id, DiaLimite) :-
    prestamo(Id, _, _, DiaSalida),
    DiaLimite is DiaSalida + 15.

% Hay un libro que nadie ha pedido en préstamo.
libro_nunca_prestado(L) :-
    libro(L),
    \+ historial(_, L),
    \+ (ejemplar(E, L, _), prestamo(_, _, E, _)).

% Todos los libros de un autor están en una misma sección.
libros_autor_misma_seccion(A) :-
    autor(A),
    seccion(S),
    forall(escribio(A, L), en_seccion(L, S)).

todos_autores_misma_seccion :-
    forall(autor(A), libros_autor_misma_seccion(A)).

% Existe un libro con más de un ejemplar en una biblioteca.
libro_varios_ejemplares(L, B) :-
    ejemplar(E1, L, B),
    ejemplar(E2, L, B),
    dif(E1, E2).

% Hay una sección donde todos los libros son de ciencias.
% Se exige que la sección contenga al menos un libro.
seccion_solo_ciencias(S) :-
    seccion(S),
    en_seccion(_, S),
    forall(en_seccion(L, S), de_ciencias(L)).

% No todos los libros de una biblioteca superan las 100 páginas.
no_todos_superan_cien_paginas(B) :-
    tiene(B, L),
    paginas(L, N),
    N =< 100.

% Existe un usuario que ha tomado todos los libros infantiles.
% Se exige que exista al menos un libro infantil.
usuario_leyo_toda_infantil(U) :-
    usuario_registrado(U),
    en_seccion(_, infantil),
    forall(
        en_seccion(L, infantil),
        historial(U, L)
    ).
```

El nombre `usuario_leyo_toda_infantil/1` se conserva del ejemplo, pero el predicado solo comprueba préstamos registrados; no demuestra que el usuario haya leído los libros. `fecha_limite/2` calcula el vencimiento, sin realizar devoluciones automáticamente. Se conserva literalmente «más de tres préstamos» mediante `N > 3`.

## 5. Consultas de ejemplo

Carga `ejercicios.pl` desde el directorio en el que lo guardaste:

```prolog
?- [ejercicios].
```

Resultados esperados con los datos anteriores:

```prolog
?- planeta(tierra).
true.

?- existe_satelite.
true.

?- cartero_mordido_por_todos(C).
C = juan.

?- unico_perro_se_muerde(P).
P = roco.

?- catalogado(don_quijote).
true.

?- puede_tomar_prestado(luis, e3).
true.

?- puede_pedir(maria).
false.

?- fecha_limite(p1, Dia).
Dia = 115.

?- central_mas_de_diez_mil.
false.
```

## 6. Cómo interpretar el programa

| Construcción | Significado |
|---|---|
| `planeta(tierra).` | Hecho |
| `Cabeza :- Cuerpo.` | La cabeza se cumple si se cumple el cuerpo |
| `X`, `Libro`, `Usuario` | Variables, con inicial mayúscula |
| `tierra`, `luis` | Constantes, con inicial minúscula |
| `,` | Conjunción: «y» |
| `_` | Variable anónima |
| `\+ Objetivo` | Negación por fallo: el objetivo no se puede demostrar |
| `forall(Condicion, Resultado)` | Comprueba el resultado para cada caso de la condición |
| `findall(X, Objetivo, Lista)` | Reúne las soluciones en una lista |
| `sort(Lista, Unicos)` | Ordena y elimina duplicados |
| `length(Lista, N)` | Obtiene la cantidad de elementos |
| `setof(X, Objetivo, [P])` | Exige que el conjunto de soluciones tenga exactamente un elemento |
| `dif(A, B)` | Exige que los términos sean distintos |

Las condiciones de existencia y cantidad **comprueban los datos cargados**; no crean individuos ni libros automáticamente. `forall/2` tiene éxito cuando no hay casos que comprobar. La negación por fallo depende de la información disponible, por lo que las consultas de disponibilidad, multas e historial suponen que esos registros están completos.

Una consulta puede producir soluciones repetidas cuando hay varias formas de demostrar el mismo resultado; por ejemplo, la Tierra gira alrededor del Sol tanto por un hecho como por una regla.
