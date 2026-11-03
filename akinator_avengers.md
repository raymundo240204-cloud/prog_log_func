# Mi minijuego de Akinator de los Avengers

Este programa hace preguntas para adivinar un personaje de los Avengers. Las opciones son Iron Man, Thor, Hulk y Capitán América.

## Código

```lisp
;; Akinator de los Avengers

(defun preguntar (pregunta)
  (format t "~a (s/n): " pregunta)
  (finish-output)
  (string-equal (string-trim " " (read-line)) "s"))

(defun akinator ()
  (format t "~%Piensa en Iron Man, Thor, Hulk o Capitán América.~%")

  (let ((personaje
          (if (preguntar "¿Tu personaje usa una armadura tecnológica?")
              "Iron Man"
              (if (preguntar "¿Tu personaje usa un martillo?")
                  "Thor"
                  (if (preguntar "¿Tu personaje se transforma en un gigante verde?")
                      "Hulk"
                      "Capitán América")))))
    (format t "~%¡Tu personaje es ~a!~%" personaje)))

(akinator)
```

## Cómo funciona

Primero se pregunta si el personaje usa una armadura tecnológica. Si la respuesta es sí, el personaje es Iron Man. Si responde que no, se pregunta si usa un martillo para saber si es Thor.

Si tampoco es Thor, se pregunta si se transforma en un gigante verde. Si responde que sí, es Hulk; si responde que no, es Capitán América.

La función `preguntar` lee la respuesta y devuelve verdadero cuando se escribe `s`. La función `akinator` usa condiciones `if` para llegar al personaje.

## Cómo ejecutarlo

Guardo el código en un archivo llamado `akinator_avengers.lisp` y lo ejecuto con:

```bash
clisp akinator_avengers.lisp
```

Respondo con `s` para sí y `n` para no. El personaje elegido debe ser uno de los cuatro de la lista.

## Ejemplo

```text
Piensa en Iron Man, Thor, Hulk o Capitán América.
¿Tu personaje usa una armadura tecnológica? (s/n): n
¿Tu personaje usa un martillo? (s/n): n
¿Tu personaje se transforma en un gigante verde? (s/n): s

¡Tu personaje es Hulk!
```
