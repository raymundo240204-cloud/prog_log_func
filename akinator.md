# Mi minijuego de Akinator en CLISP

Este programa es una versión sencilla de Akinator. El usuario piensa en Goku, Naruto, Mario o Bob Esponja y el programa hace preguntas para adivinar cuál eligió.

## Código

```lisp
;; Minijuego de Akinator

(defun preguntar (pregunta)
  (format t "~a (s/n): " pregunta)
  (finish-output)
  (string-equal (string-trim " " (read-line)) "s"))

(defun akinator ()
  (format t "~%Piensa en Goku, Naruto, Mario o Bob Esponja.~%")

  (let ((personaje
          (if (preguntar "¿Tu personaje es de un anime?")
              (if (preguntar "¿Tu personaje es un ninja?")
                  "Naruto"
                  "Goku")
              (if (preguntar "¿Tu personaje vive bajo el agua?")
                  "Bob Esponja"
                  "Mario"))))
    (format t "~%¡Tu personaje es ~a!~%" personaje)))

(akinator)
```

## Cómo funciona

Primero se pregunta si el personaje es de un anime. Si la respuesta es sí, se pregunta si es ninja para elegir entre Naruto y Goku. Si la respuesta es no, se pregunta si vive bajo el agua para elegir entre Bob Esponja y Mario.

La función `preguntar` muestra una pregunta y lee la respuesta. Devuelve verdadero si se escribe `s`. La función `akinator` usa condiciones `if` para elegir el personaje y mostrar el resultado.

## Cómo ejecutarlo

Guardo el código en un archivo llamado `akinator.lisp` y lo ejecuto desde la terminal con este comando:

```bash
clisp akinator.lisp
```

Para responder uso `s` para sí y `n` para no.

## Ejemplo

```text
Piensa en Goku, Naruto, Mario o Bob Esponja.
¿Tu personaje es de un anime? (s/n): s
¿Tu personaje es un ninja? (s/n): n

¡Tu personaje es Goku!
```
