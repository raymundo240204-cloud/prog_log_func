# Ejercicios

```lisp
(defparameter *sueldo-base* 40000)
(defparameter *anios* 0)
(defparameter *porcentaje* 0)
(defparameter *aumento* 0)
(defparameter *sueldo-final* 0)

(defun calcular-sueldo ()
  (format t "Ingrese los anios de antiguedad: ")
  (finish-output)
  (setf *anios* (read))

  (when (< *anios* 0)
    (format t "La antiguedad no puede ser negativa.~%"))

  (unless (< *anios* 0)
    (setf *porcentaje*
          (if (> *anios* 10)
              10
              (if (> *anios* 5)
                  7
                  (if (> *anios* 3)
                      5
                      3))))

    (setf *aumento*
          (* *sueldo-base* (/ *porcentaje* 100)))

    (setf *sueldo-final*
          (+ *sueldo-base* *aumento*))

    (format t "~%Sueldo base: ~,2F euros~%" *sueldo-base*)
    (format t "Porcentaje de aumento: ~D%~%" *porcentaje*)
    (format t "Aumento: ~,2F euros~%" *aumento*)
    (format t "Sueldo anual final: ~,2F euros~%" *sueldo-final*)))

(calcular-sueldo)
```

