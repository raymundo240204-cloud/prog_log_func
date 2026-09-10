# Áreas de figuras geométricas

```lisp
; 1. Rectángulo
(defun area-rectangulo (base altura)
  (* base altura))

; 2. Triángulo
(defun area-triangulo (base altura)
  (/ (* base altura) 2))

; 3. Círculo
(defun area-circulo (radio)
  (* 3.141592653589793 radio radio))

; 4. Rombo
(defun area-rombo (diagonal-mayor diagonal-menor)
  (/ (* diagonal-mayor diagonal-menor) 2))

; 5. Trapecio
(defun area-trapecio (base-mayor base-menor altura)
  (/ (* (+ base-mayor base-menor) altura) 2))

; 6. Paralelogramo
(defun area-paralelogramo (base altura)
  (* base altura))

; 7. Pentágono regular
(defun area-pentagono (lado apotema)
  (/ (* 5 lado apotema) 2))

; 8. Hexágono regular
(defun area-hexagono (lado apotema)
  (/ (* 6 lado apotema) 2))

; 9. Elipse
(defun area-elipse (semieje-mayor semieje-menor)
  (* 3.141592653589793 semieje-mayor semieje-menor))
```
