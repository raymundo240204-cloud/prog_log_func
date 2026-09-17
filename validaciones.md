# Validaciones

```lisp
(deffunction validar-edad-cine (?edad)
   (if (integerp ?edad)
      then
      (if (< ?edad 0)
         then
         (printout t "Error: Edad negativa" crlf)
         (return FALSE)
         else
         (if (> ?edad 120)
            then
            (printout t "Error: Edad falsa" crlf)
            (return FALSE)
            else
            (if (< ?edad 13)
               then
               (printout t "AA (infantil)" crlf)
               else
               (if (< ?edad 18)
                  then
                  (printout t "B (adolescentes)" crlf)
                  else
                  (printout t "B15/C (adultos)" crlf)
               )
            )
            (return TRUE)
         )
      )
      else
      (printout t "Error: La edad debe ser un entero" crlf)
      (return FALSE)
   )
)

(deffunction avisar-password (?clave)
   (if (< (str-length ?clave) 8)
      then
      (printout t "Aviso: clave corta (< 8)" crlf)
   )

   (if (or (eq ?clave "12345678")
           (eq ?clave "password"))
      then
      (printout t "Aviso: clave demasiado comun" crlf)
   )

   (if (and (> (str-length ?clave) 0)
            (eq ?clave (lowcase ?clave)))
      then
      (printout t "Aviso: no hay mayusculas" crlf)
   )

   (return TRUE)
)
```

